# 第2课 课后作业


## 第1题 转换为bit位 Num2Bits

```
pragma circom 2.1.4;

include "circomlib/poseidon.circom";
// include "https://github.com/0xPARC/circom-secp256k1/blob/master/circuits/bigint.circom";

template NumToBit (n) {
    signal input in;

    signal output b[n];

    var tmp = 0;
    for (var i = 0; i < n ; i ++){
        b[i] <-- (in \ 2 ** i) % 2;
        0 === b[i] * (1 - b[i]);
        tmp += b[i] * (2 ** i);
        
    }
    in === tmp;

}

component main  = NumToBit(6);

/* INPUT = {
    "in": "11"
} */

```


## 第2题 判零 IsZero
```
pragma circom 2.1.4;

include "circomlib/poseidon.circom";
// include "https://github.com/0xPARC/circom-secp256k1/blob/master/circuits/bigint.circom";

template IsZero () {
    signal input in;
    signal output o;

    signal inv;
    inv <-- in!=0 ? 1/in : 0;
    o <== -in*inv +1;
    in*o === 0;
    
}

component main  = IsZero();

/* INPUT = {
    "in": "5"
} */
```


## 第3题 相等 IsEqual
```
pragma circom 2.1.4;

include "circomlib/poseidon.circom";
// include "https://github.com/0xPARC/circom-secp256k1/blob/master/circuits/bigint.circom";

template IsEqual () {
    signal input in[2];
    signal output out;

    signal tmp;
    signal isz;
    isz <== in[1] - in[0];
    tmp <-- isz!=0 ? 1/isz : 0;
    out <== -isz*tmp +1;
    isz*out === 0;
}

component main  = IsEqual();

/* INPUT = {
    "in": ["5", "4"]
} */
```

## 第4题 选择器 Selector
```
pragma circom 2.1.4;
template NumToBit (n) {
    signal input in;

    signal output b[n];

    var tmp = 0;
    for (var i = 0; i < n ; i ++){
        b[i] <-- (in \ 2 ** i) % 2;
        0 === b[i] * (1 - b[i]);
        tmp += b[i] * (2 ** i);
        
    }
    in === tmp;

}


template IsEqual () {
    signal input in[2];
    signal output out;

    signal tmp;
    signal isz;
    isz <== in[1] - in[0];
    tmp <-- isz!=0 ? 1/isz : 0;
    out <== -isz*tmp +1;
    isz*out === 0;
}

template LessThan(n) {
    assert(n <= 252);
    signal input in[2];
    signal output out;

    component n2b = NumToBit(n+1);
    n2b.in <== in[0]+ (1<<n) - in[1];

    out <== 1-n2b.b[n];
}

template CalculateTotal(n) {
    signal input in[n];
    signal output out;

    signal total[n];

    total[0] <== in[0];

    for (var i = 1; i < n; i++) {
        total[i] <== total[i-1] + in[i];
    }

    out <== total[n-1];
}

template Selector(nChoices) {
    signal input in[nChoices];
    signal input index;
    signal output out;


    component lessThan = LessThan(4);
    lessThan.in[0] <== index;
    lessThan.in[1] <== nChoices;
    lessThan.out === 1;

    component calcTotal = CalculateTotal(nChoices);
    component isequal[nChoices];

    for (var i = 0; i < nChoices; i ++) {
        isequal[i] = IsEqual();
        isequal[i].in[0] <== i;
        isequal[i].in[1] <== index;

        calcTotal.in[i] <== isequal[i].out * in[i];
    }

    out <== calcTotal.out;
}
component main = Selector(8);
/* INPUT = {
        "in": ["14","55","6","109","177","2","3","68"],
        "index":"8"
    } */
```




## 第5题 判负 IsNegative
```

```




## 第6题 少于 LessThan
```
pragma circom 2.1.4;

include "circomlib/poseidon.circom";

template NumToBit (n) {
    signal input in;

    signal output b[n];

    var tmp = 0;
    for (var i = 0; i < n ; i ++){
        b[i] <-- (in \ 2 ** i) % 2;
        0 === b[i] * (1 - b[i]);
        tmp += b[i] * (2 ** i);
        
    }
    in === tmp;

}

template LessThan(n) {
    assert(n <= 252);
    signal input in[2];
    signal output out;

    component n2b = NumToBit(n+1);
    n2b.in <== in[0]+ (1<<n) - in[1];

    out <== 1-n2b.b[n];
}

component main = LessThan(222);

/* INPUT = {
    "in": ["9","6"]
} */
```



## 第7题 整数除法 IntegerDivide
```

```

