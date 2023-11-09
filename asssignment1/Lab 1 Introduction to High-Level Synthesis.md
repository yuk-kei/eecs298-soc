# Lab 1: Introduction to High-Level Synthesis

## PART I: Vector Addition Example

`pl_vecadd.cpp`

``` c ++
#include "ap_int.h"
#include "ap_fixed.h"
#include "hls_math.h"

void pl_vecadd(float a[1024], float b[1024], float c[1024])
{
  #pragma HLS INTERFACE m_axi port=a offset=slave bundle=data0
  #pragma HLS INTERFACE s_axilite register port=a bundle=ctrl
  #pragma HLS INTERFACE m_axi port=b offset=slave bundle=data1
  #pragma HLS INTERFACE s_axilite register port=b bundle=ctrl
  #pragma HLS INTERFACE m_axi port=c offset=slave bundle=data2
  #pragma HLS INTERFACE s_axilite register port=c bundle=ctrl
  #pragma HLS INTERFACE s_axilite register port=return bundle=ctrl
  for (int i = 0; i < 1024; i += 1)
  {
    #pragma HLS pipeline
    c[i] = a[i] + b[i];
  }

}
```



### Vitis HLS synthesis summary

### Vivado post-implementation summary



## PART II: Matrix Multiplication



``` c++
void matmul_plain(int A[16][16], int B[16][16], int AB[16][16]) {
    for (int i = 0; i < 16; i++) {
        for (int j = 0; j < 16; j++) {
            int sum = 0;
            for (int k = 0; k < 16; k++) {
                sum += A[i][k] * B[k][j];
            }
            AB[i][j] = sum;
        }
    }
}
```



``` c++
void matmul_optimized(int A[16][16], int B[16][16], int AB[16][16]) {
    #pragma HLS ARRAY_PARTITION variable=A block factor=4 dim=2
    #pragma HLS ARRAY_PARTITION variable=B block factor=4 dim=1
    #pragma HLS ARRAY_PARTITION variable=AB block factor=4 dim=2

    for (int i = 0; i < 16; i++) {
        for (int j = 0; j < 16; j++) {
            #pragma HLS PIPELINE
            int sum = 0;
            for (int k = 0; k < 16; k++) {
                #pragma HLS UNROLL factor=4
                sum += A[i][k] * B[k][j];
            }
            AB[i][j] = sum;
        }
    }
}
```

