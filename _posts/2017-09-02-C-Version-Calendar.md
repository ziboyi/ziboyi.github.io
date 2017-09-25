---
layout: post
title: "A Calendar writen by C programing language(Leap Year Judging Included)"
date: 2017-09-02
excerpt: "C version Calendar"
tags: [C]
comments: true
---

```c
#include <stdio.h>

int isLeapYear(int year)
{
    if ((year % 400 == 0 || year % 100 != 0) && year % 4 == 0)
        return 1;
    else return 0;
}

int days(int year, int month)
{
    if (month == 2 && isLeapYear(year)) {
        return 29;
    } else {
        switch(month) {
            case 1: return 31;
            case 2: return 28;
            case 3: return 31;
            case 4: return 30;
            case 5: return 31;
            case 6: return 30;
            case 7: return 31;
            case 8: return 31;
            case 9: return 30;
            case 10: return 31;
            case 11: return 30;
            case 12: return 31;
            default: printf("Error: no such month!\n"); return 0;
        }
    }
}

int firstDays(int year, int month)
{
    int y = year;
    int m = month;
    int d = 1;
    int y0 = y - (14 - m) / 12;
    int x = y0 + y0 / 4 - y0 / 100 + y0 / 400;
    int m0 = m + 12 *((14 - m) / 12) - 2;
    int d0 = (d + x + (31 * m0) / 12) % 7;
    return d0;
}


int main()
{
    int i, year, month, whichDay;
    printf("Please enter year and month:\n");
    scanf("%d %d", &year, &month);
    printf("\nS\tM\tT\tW\tT\tF\tS\n");

    for (i = 0; i < (whichDay = firstDays(year, month)); i++) {
        printf("\t");
    }

    for (i = 0; i < days(year, month); i++) {
        if ((whichDay++) == 7) {
            printf("\n");
            whichDay = 1;
        }
        printf("%d\t", i + 1);
    }
    printf("\n");

    return 0;

}


```

The result should be:


<figure class="third">
	<img src="https://ziboyi.github.io/assets/img/calender.png">
</figure>
