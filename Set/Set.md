#### HashSet ####
HashSet组合了HshMap。将value都赋值一个固定值。方法的使用都是通过包装HashMap的底层方法


#### TreeSet ####
TreeSet组合了TreeMap。同样，value也赋值一个固定值。在复用TreeMap时，有两个思路。
* 直接包装TreeMap的方法。
* TreeSet定义了接口规范，让TreeMap帮助去实现。因为有些复杂的操作TreeSet实现困难，并且并不了解TreeMap的底层实现，于是让TreeMap帮忙实现。
