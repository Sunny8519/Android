在时间格式字符串中，如下字符会被解释成具体时间，字符串中的某些字母或单词不想被解释的话，可以使用单引号引起来，一个单引号可以使用双引号表示。

![DateFormat](https://upload-images.jianshu.io/upload_images/5231076-fe5249850b254e6b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

示例：
美国太平洋时间2001-07-04 12:08:56

![DateFormat_1](https://upload-images.jianshu.io/upload_images/5231076-5c760aeb8dc06ad0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

yyyy：代表年(不区分大小写，一般为小写)，假设年份为2017
"y","yyy","yyyy"匹配的都是4位完整的年，如2017
"yy"匹配的是年份的后两位，如17
超过4位，会在年份的前面补加0，如"yyyyy"对应"02017"

MM：代表月份，只能大写，假设月份为9
"M"表示"9"
"MM"表示"09"
"MMM"表示"Sep"
"MMMM"表示"September"

dd：代表日，只能小写

hh：代表时，区分大小写，大写表示二十四小时制，小写表示十二小时制

mm：代表分，只能小写

ss：代表秒，只能小写

E：代表星期，只能大写，假设为Sunday
"E","EE","EEE"对应"Sun"
"EEEE"对应"Sunday",超出4位仍然对应"Sunday"

a：代表上午还是下午，如果是上午就对应"AM",如果是下午就对应"PM"