Object[数组](https://so.csdn.net/so/search?q=数组&spm=1001.2101.3001.7020)（可以存储不同类型数据)

Object[] a= {1001,"ad",12,"qwd"};
Object[][] aa=new Object[3][];
并且能使用Arrays.toString()等方法进行数组工具操作

//上面的代码省略
Integer[] a = new Integer[6];
Object[] array = set.toArray();    //这里会生成一个Object[]
a = Arrays.copyOfRange(array, 0, 5,Integer[].class);

