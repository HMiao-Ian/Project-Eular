# Problem 2 in Project Eular

## Problem discription

Each new term in the Fibonacci sequence is generated by adding the previous two terms. By starting with 1 and 2, the first 10 terms will be:
> $1, 2, 3, 5, 8, 13, 21, 34, 55, 89\cdots$

By considering the terms in the Fibonacci sequence whose values do not exceed four million, find the sum of the even-valued term.

## Idea

Well, my idea is divided into two parts:

* fibonacci function implementation;
* examine every fibonacci item whether it's even or not.

### Fibonacci function with recurrence

```C
long fibonacci(int item)
{
    if (item > 0 && item <= 3)
        return item;
    else
        return fibonacci(item-1)+fibonacci(item-2);
}
```

### Fibonacci function with non-recurrence

```C
long fib(int item)
{
    int a = 1, b = 2, c = 3;
    int flag = 0;
    long fib = 0;

    if (item > 0 && item <= 3)
        return item;
    else
    {
        for(int i = 4; i < item; i++)
        {
            switch(flag)
            {
                case 0:
                    fib = a + b;
                    c = fib;
                    break;
                case 1:
                    fib = b + c;
                    a = fib;
                    break;
                case 2:
                    fib = a + c;
                    b = fib;
                default:
                    break;
            }
        }
    }

    return fib;
    /*
    Now, a, b, c represent the first, second, third item in fibonacci sequence.
    They are aimed to record the former two items because the computation only need the former two items.
    flag is aimed to indicate that the current fibonacci item should assign itself to a OR b OR c.
    fib is just the fibonacci(item).
    */
}
```

Let me show you how flag variable in non-recurrence work. By the way, I didn't refer to anyone when I writen this program. So maybe this program is not mature and I will modify it if I meet a better one.

<font size = 6>

$$\mathbf{X}=\begin{bmatrix}
{^1}1&{^2}2&{^3}3\\
{^4}5&{^5}8&{^6}13\\
{^7}21&{^8}34&{^9}55
\end{bmatrix}
$$
</font>

This  matrix **X** represents the first nine items and the top left number is the fibonacci item number. I choose to matrix to express fib sequence just because of the simpility, not special cause. Now, you remember that,
> $a: fib(1),\quad b:fib(2),\quad c:fib(3)$

Now I calculate the 4th item,
> $fib(4)=fib(3)+fib(2)=b+c$
>
> $a\leftarrow fib(4)$
---
> $a:fib(4),\quad b:fib(2),\quad c:fib(3)$
>
> $fib(5)=fib(4)+fib(3)=a+c$
>
> $b\gets fib(5)$
---
> $a:fib(4),\quad b:fib(5),\quad c:fib(3)$
>
> $fib(6)=fib(5)+fib(4)=a+b$
>
> $c\gets fib(6)$

Then I get the 4th, 5th, 6th item. And it's easy to understand that the assignment rule is that the later item assign itself to former item **in the same column**. That's what **flag** do. And you can calculate 7th, 8th, 9th to examine this rule.

But I get confused because when I calculate fib(45)=

By using this function, I get
> $fib(32)=3524578\qquad fib(33)=5702887$

So I just find even-valued item from first 32 items. And another interesting thing is that all the even items in fibonacci sequence are in the same column -- column 2. So add up all these even values to get the answer.

## Answer

> 4613732

## Question

I get confused because when I calculate
> $fib(44)=1134903170\qquad fib(45)=1836311903$

but the 46th is negative,
> $fib(46)=-1323752223\quad it\quad should\quad be\quad fib(46)=2971215073$

In my laptop, codeblocks, "long" occypy 4 bytes, and $2^{32}-1=4294967296 > fib(46)$. So it should overflow. Then it must be the non-recurrence fibonacci function's error. And I'll continue to find where exactly the error is.

[EOF]
