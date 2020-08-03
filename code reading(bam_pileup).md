# bam file pileup -- code reading

`mplp`: mpileup 产生pileup format txt  
`hts`:
`hdr`: header
`ret`: main函数return值
conf: configuration for this pileup
fn: filenames
fn_idx: index filenames
bam1_t


## C语言相关
1. strspn（返回字符串中第一个不在指定字符串中出现的字符下标）  
size_t strspn (const char * s,const char * accept);
函数说明 strspn()从参数s 字符串的开头计算连续的字符，而这些字符都完全是accept 所指字符串中的字符。简单的说，若strspn()返回的数值为n，则代表字符串s 开头连续有n 个字符都是属于字符串accept内的字符。  
返回值 返回字符串s开头连续包含字符串accept内的字符数目。  

2. fgets 每次读取指定大小的文件到buf里面
函数原型
char * fgets(char * str, int n, FILE * stream);  
参数  
    str-- 这是指向一个字符数组的指针，该数组存储了要读取的字符串。  
    n-- 这是要读取的最大字符数（包括最后的空字符）。通常是使用以 str 传递的数组长度。  
    stream-- 这是指向 FILE 对象的指针，该 FILE 对象标识了要从中读取字符的流。  
功能  
从指定的流 stream 读取一行，并把它存储在str所指向的字符串内。当读取(n-1)个字符时，或者读取到换行符时，或者到达文件末尾时，它会停止，具体视情况而定。  

3. stat()函数：获取文件状态
定义函数：int stat(const char * file_name, struct stat * buf);
函数说明：stat()用来将参数file_name 所指的文件状态, 复制到参数buf 所指的结构中。

4. realloc() 函数用来重新分配内存空间，其原型为：  
    void* realloc (void* ptr, size_t size);   
【参数说明】ptr 为需要重新分配的内存空间指针，size 为新的内存空间的大小。  

5. strdup()  
extern char * strdup(char * s);
strdup()在内部调用了malloc()为变量分配内存，不需要使用返回的字符串时，需要用free()释放相应的内存空间，否则会造成内存泄漏。  
返回一个指针,指向为复制字符串分配的空间;如果分配空间失败,则返回NULL值。

6. memset()
void * memset(void * s, int ch, size_t n);
函数解释：将s中当前位置后面的n个字节 （typedef unsigned int size_t ）用 ch 替换并返回 s 。
memset：作用是在一段内存块中填充某个给定的值，它是对较大的结构体或数组进行清零操作的一种最快方法

7. getopt_long()  
getopt被用来解析命令行选项参数。
getopt_long支持长选项的命令行解析，使用man getopt_long，得到其声明如下：
int getopt_long(int argc, char * const argv[],const char * optstring, const struct option * longopts,int * flag);
函数中的argc和argv通常直接从main()的两个参数传递而来。optsting是选项参数组成的字符串


## 函数
khash_str2int_set 
strtok_r
faidx_fetch_seq64 获取ref

## typedef struct
```C
//
typedef struct {
    int min_mq, flag, min_baseQ, capQ_thres, max_depth, max_indel_depth, fmt_flag, all, rev_del;
    int rflag_require, rflag_filter;
    int openQ, extQ, tandemQ, min_support; // for indels
    double min_frac; // for indels
    char *reg, *pl_list, *fai_fname, *output_fname;
    faidx_t *fai;
    void *bed, *rghash, *auxlist;
    int argc;
    char **argv;
    char sep, empty;
    sam_global_args ga;
} mplp_conf_t;
```

```C
//ref 相关
typedef struct {
    char *ref[2];
    int ref_id[2];
    hts_pos_t ref_len[2];
} mplp_ref_t;
```

```C
typedef struct {
    samFile *fp;
    hts_itr_t *iter;
    sam_hdr_t *h;
    mplp_ref_t *ref;
    const mplp_conf_t *conf;
} mplp_aux_t;
```

```C
typedef struct {
    int n;
    int *n_plp, *m_plp;
    bam_pileup1_t **plp;
} mplp_pileup_t;
```

#### bam2bcf.h  
结构体  
bcf_callaux_t  
bcf_callret1_t  
bcf_call_t  


#### htslib kstring.h
```c
typedef struct kstring_t {
    size_t l, m;
    char *s;
} kstring_t;
```


```


struct stat sb;??????????

## sam pileup

说明解释见 bam.h * pileup APIs *

