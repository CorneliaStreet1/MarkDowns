# PA1

## 关于多线程编译

待补

## 关于指令集

选择了Riscv

## 阅读源代码

- 先从main开始阅读
- nemu-main.c

```c
#include <common.h>

void init_monitor(int, char *[]);
void am_init_monitor();
void engine_start();
int is_exit_status_bad();

int main(int argc, char *argv[]) {
  /* Initialize the monitor. */
#ifdef CONFIG_TARGET_AM//条件编译，在编译时确定的条件语句,用于确定某些语句是否编译，当CONFIG_TARGET_AM无定义时就不编译
  am_init_monitor();
#else
  init_monitor(argc, argv);
#endif

  /* Start engine. */
  engine_start();

  return is_exit_status_bad();
}
```

- 阅读main得知要去阅读监视器相关

- NEMU在开始运行的时候, 首先会调用`init_monitor()`函数(在`nemu/src/monitor/monitor.c`中定义) 来进行一些和monitor相关的初始化工作

- `init_monitor()`函数  在`nemu/src/monitor/monitor.c`中定义

- `parse_args()`, `init_rand()`, `init_log()`和`init_mem()`并没有什么深奥的内容, 直接RTFSC就行.

- ```c
  /*一些必要的定义补充*/
  /****************1，struct option************/
  struct option
  {
    const char *name;//长选项的名字
    /* has_arg can't be an enum because some compilers complain about
       type mismatches in all the code that assumes it is an int.  */
    int has_arg;//关于这个的定义见下，长选项所需的参数个数
    int *flag;/*flag指定如何为长选项返回结果，如果flag为NULL，则getopt_long()直接返回val。否则getopt_long()返回0，flag则指向一个变量，如果选项被找到则这个变量被设置为val的值，没有找到则不改变它的值*/
    int val;//要被返回的值，或要被加载入被flag指向的变量的值
  };
  /* Names for the values of the 'has_arg' field of 'struct option'.  */
  #define no_argument		0//选项无需参数
  #define required_argument	1//选项需要1个参数
  #define optional_argument	2//选项接受可选参数
  /********2.getopt_long()**************/
  #include <getopt.h>
  int getopt_long(int argc, char * const argv[],const char *optstring,
                  const struct option *longopts, int *longindex);
  /*getopt_long()和getopt()工作起来差不多，只不过它接受以"--”开头的长选项，longopts是指向option结构数组的第一个元素的指针*/
  extern char *optarg;
  extern int optind, opterr, optopt;
  int getopt(int argc, char * const argv[],const char *optstring);
  /*这个函数对命令行参数进行语法分析，这个函数的参数，argc为参数个数，argv为保存有参数的数组，和从main传入的的一样。一个以"-”开头并且不仅仅是“-”或者“--”的argv中的元素被视为一个选项元素，这个元素(除去开头的“-”)是选项字符(option characters)。如果getopt()被重复调用，它依次返回每个选项元素(option elements)的每个选项字符。如果没有更多选项了，getopt()返回-1。参数 optstring为选项字符串， 告知 getopt()可以处理哪个选项以及哪个选项需要参数，如果选项字符串里的字母后接着冒号“:”，则表示还有相关的参数，全域变量optarg 即会指向此额外参数。如果在处理期间遇到了不符合optstring指定的其他选项getopt()将显示一个错误消息，并将全域变量optopt设为“?”字符，如果不希望getopt()打印出错信息，则只要将全域变量opterr设为0即可。
  ```

  

```c
#include <isa.h>
#include <memory/paddr.h>

void init_rand();
void init_log(const char *log_file);
void init_mem();
void init_difftest(char *ref_so_file, long img_size, int port);
void init_device();
void init_sdb();
void init_disasm(const char *triple);

static void welcome() {
  Log("Trace: %s", MUXDEF(CONFIG_TRACE, ASNI_FMT("ON", ASNI_FG_GREEN), ASNI_FMT("OFF", ASNI_FG_RED)));
  IFDEF(CONFIG_TRACE, Log("If trace is enabled, a log file will be generated "
        "to record the trace. This may lead to a large log file. "
        "If it is not necessary, you can disable it in menuconfig"));
  Log("Build time: %s, %s", __TIME__, __DATE__);
  printf("Welcome to %s-NEMU!\n", ASNI_FMT(str(__GUEST_ISA__), ASNI_FG_YELLOW ASNI_BG_RED));
  printf("For help, type \"help\"\n");
  Log("Exercise: Please remove me in the source code and compile NEMU again.");
  assert(0);
}

#ifndef CONFIG_TARGET_AM
#include <getopt.h>

void sdb_set_batch_mode();

static char *log_file = NULL;
static char *diff_so_file = NULL;
static char *img_file = NULL;
static int difftest_port = 1234;

static long load_img() {
  if (img_file == NULL) {
    Log("No image is given. Use the default build-in image.");
    return 4096; // built-in image size
  }

  FILE *fp = fopen(img_file, "rb");
  Assert(fp, "Can not open '%s'", img_file);

  fseek(fp, 0, SEEK_END);
  long size = ftell(fp);

  Log("The image is %s, size = %ld", img_file, size);

  fseek(fp, 0, SEEK_SET);
  int ret = fread(guest_to_host(RESET_VECTOR), size, 1, fp);
  assert(ret == 1);

  fclose(fp);
  return size;
}

static int parse_args(int argc, char *argv[]) {
    /*定义了一个struct option的数组，struct option的定义见补充代码*/
  const struct option table[] = {
    {"batch"    , no_argument      , NULL, 'b'},
    {"log"      , required_argument, NULL, 'l'},
    {"diff"     , required_argument, NULL, 'd'},
    {"port"     , required_argument, NULL, 'p'},
    {"help"     , no_argument      , NULL, 'h'},
    {0          , 0                , NULL,  0 },
  };
  int o;
    /*getopt_long():man 3 getopt_long*/
  while ( (o = getopt_long(argc, argv, "-bhl:d:p:", table, NULL)) != -1) {
    switch (o) {
      case 'b': sdb_set_batch_mode(); break;
      case 'p': sscanf(optarg, "%d", &difftest_port); break;
      case 'l': log_file = optarg; break;
      case 'd': diff_so_file = optarg; break;
      case 1: img_file = optarg; return optind - 1;
      default:
        printf("Usage: %s [OPTION...] IMAGE [args]\n\n", argv[0]);
        printf("\t-b,--batch              run with batch mode\n");
        printf("\t-l,--log=FILE           output log to FILE\n");
        printf("\t-d,--diff=REF_SO        run DiffTest with reference REF_SO\n");
        printf("\t-p,--port=PORT          run DiffTest with port PORT\n");
        printf("\n");
        exit(0);
    }
  }
  return 0;
}

void init_monitor(int argc, char *argv[]) {
  /* Perform some global initialization. */

  /* Parse arguments. */
  parse_args(argc, argv);//对参数进行语法分析

  /* Set random seed. */
  init_rand();//为什么需要建立随机种子？

  /* Open the log file. */
  init_log(log_file);

  /* Initialize memory. */
  init_mem();

  /* Initialize devices. */
  IFDEF(CONFIG_DEVICE, init_device());

  /* Perform ISA dependent initialization. */
  init_isa();//

  /* Load the image to memory. This will overwrite the built-in image. */
  long img_size = load_img();

  /* Initialize differential testing. */
  init_difftest(diff_so_file, img_size, difftest_port);

  /* Initialize the simple debugger. */
  init_sdb();

  IFDEF(CONFIG_ITRACE, init_disasm(
    MUXDEF(CONFIG_ISA_x86,     "i686",
    MUXDEF(CONFIG_ISA_mips32,  "mipsel",
    MUXDEF(CONFIG_ISA_riscv32, "riscv32",
    MUXDEF(CONFIG_ISA_riscv64, "riscv64", "bad")))) "-pc-linux-gnu"
  ));

  /* Display welcome message. */
  welcome();
}
#else // CONFIG_TARGET_AM
static long load_img() {
  extern char bin_start, bin_end;
  size_t size = &bin_end - &bin_start;
  Log("img size = %ld", size);
  memcpy(guest_to_host(RESET_VECTOR), &bin_start, size);
  return size;
}

void am_init_monitor() {
  init_rand();
  init_mem();
  init_isa();
  load_img();
  IFDEF(CONFIG_DEVICE, init_device());
  welcome();
}
#endif
```

### 关于错误修复

把`nemu/src/monitor/monitor.c`中的line20和line21注释掉即可
