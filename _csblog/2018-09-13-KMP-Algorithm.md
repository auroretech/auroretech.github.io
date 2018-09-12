---
layout: post_for_cs
title:  "ģʽ��ƥ���㷨���"
date:   2018-09-13
excerpt: "ģʽ��ƥ���㷨�Ļ���˼��ͼ򵥵Ľ���..."
image: "/images/2018-9-13-kmp-covergirl.jpg"
comments: true
---

## ģʽ��ƥ���㷨˼��
> [Source Here](http://www.auroretech.com/computer-science/KMP-Algorithm/)
#### �򵥵�ģʽ��ƥ���㷨
> ���ü���ָ��$i$��$j$�ֱ�ָʾģʽ�����������ַ�λ��
����ѭ���ж�$string1[ i ] == string2[ j ]$,������Ƚ�֮����ַ�������������
��һ���ַ��������жϡ�

- �������ַ�ʱ����ʱ�临�ӶȽϺ�����ɴﵽ$O(n+m)$
- ������$01$��ʱ����ʱ�临�Ӷ������ɴ�$O(n \times m)$��һ������ѡ��

#### KMP���
> $\mathcal{D.E.Knuth \space\space\space J.H.Morris \space\space\space V.R.Pratt}$

- ʱ�临�Ӷ�$O(n+m)$
- ���㷨��˼��Ľ����ڣ�ÿ����һ��ƥ����̳����ַ�����ʱ��**�������ָ��**�����������Ѿ��õ��ġ�����ƥ�䡱��ģʽ���ҡ�������������Զ��һ�ξ����˺󣬼������бȽ�
> ���⣺**����**�еĵ�$i$���ַ�($i$ ָ�벻����)Ӧ��**ģʽ��**�е��ĸ��ַ��ٱȽϣ�

- ����Ӧ��ģʽ�е�k(k<j)���ַ������Ƚ���
$$ \prime P_1 P_2 \dots P_{k-1}\prime = \prime S_1 S_2 S_3 \dots S_{i-1} \prime$$
���Ѿ��õ��ġ����ֽ������
$$ \prime P_{i-k+1} P_{j-k+2} \dots P_{j-1}\prime = \prime S_{i-k+1} S_{i-k+2} \dots S_{i-1} \prime$$
$$ \Longrightarrow   \prime P_1 P_2 \dots P_{k-1}\prime = \prime P_{i-k+1} P_{j-k+2} \dots P_{j-1}\prime $$
�ʣ�
$$ next[j] = \begin{cases}0&\text{�� j=1} \\Max\{k\bracevert 1<k<j �� \prime P_1 P_2 \dots P_{k-1}\prime = \prime P_{i-k+1} P_{j-k+2} \dots P_{j-1}\prime & \text{���˼��ϲ���ʱ} \\1 \text{�������}\end{cases}$$
$\Longrightarrow$
��$S_i == P_j$ʱ����$i$��$j$����������$i$���䣬$j$����$next[j]$λ�ü����Ƚϣ���ʱ�����ֽ����
1. $j$�˵�ĳ��$next$ֵʱ�ַ��Ƚ���ȣ���ָ������$1$������ƥ�䡣
2. $j$����ֵΪ��(����һ���ַ���ƥ��ʧ��)����**����**����һ���ַ����ģʽ����ƥ�䡣
> �����Ǵ��������
```
int index_KMp(string S, string T, int position)
{
    i = pos;//ӦҪ���posλ�ÿ�ʼƥ��
    j = 1;//ģʽ����1��ʼ����
    while(i <= length(S) && j<= length(T))
    {
        if(j == 0 || S[i]==T[j]){++i; ++j;}//j=0ʱ����ʹi,j����
        else j = next[j];
    }
    if(j > length(T))return i-length(T);//��ƥ��ɹ�ʱ��j���ģʽ�����ȴ�1
    else return 0;
}
```
![KMP](http://www.auroretech.com/images/2018-9-12-kmp.jpg)
> �ο��������ݽṹ����C���԰棩 ��ε������ΰ�����

> $TODO��Next$��ֵ��$KMP$�Ĺؼ�������ƪ������