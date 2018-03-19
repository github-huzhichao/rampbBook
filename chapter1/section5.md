# ADO.NET中的五个主要对象

**Connection**：主要用来开启程序和数据库之间的连接，没有利用Connection对象连接数据库，是无法从数据库中取得数据的。Close()和Dispose()的区别就是Close以后还可以Open，但是Dispose是释放了连接，要操作数据库就要重新连接数据库。

**Command**：主要用来对数据库发出一些指令，例如可以对数据库发出增删改查的指令，或者调用存在数据库中的存储过程等。这个对象是建立在Connection对象之上的，也就是Command对象需要连接到数据库之后才可以操作数据库中的数据。

**DataAdapter**：主要是在数据源以及DataSet之间执行数据库传输工作，它可以透过Command对象下达命令后，然后将取得的数据通过DataAdapter对象调用Fill()方法填充到DataSet对象中。

**DataSet**：这个对象可以视为一个暂存区(Cache)，可以把从数据库中所查询到的数据保留起来，甚至可以将整个数据库显示出来，DataSet是放在内存中的，DataSet的能力不只是可以存储多个Table而已，还可以透过DataAdapter对象取得一些例如主键等的数据表结构，并可以记录数据表间的关联。DataSet对象可以说是ADO.Net中重量级的对象，这个对象架构在DataAdapter对象上，本身不具备和数据源沟通的能力；也就是说我们是将DataAdapter对象当作DataSet对象以及数据源间传输数据的桥梁。DataSet包含若干DataTable、DataTable包含若干DataRow。

**DataReader**：当我们只需要循序的读取数据而不需要其他操作时，可以使用DataReader对象。DataReader对象只是一次一次向下循序读取数据源中的数据，这些数据是存在数据库服务器中的，而不是一次性加载到程序的内存中的，只能(通过游标)读取当前行的数据，而且这些数据是只读的，并不允许其他的操作。因为DataReader在读取数据的时候限制了每次只读取一行，而且只能只读，所以使用起来不但节省资源而且效率很高。使用DataReader对象除了效率较好之外，因为不用把数据全部传回，故可以降低网路的负载。

总结：ADO.NET 使用Connection 对象来连接数据库，使用Command 或DataAdapter 对象来执行SQL 语句，并将执行的结果返回给DataReader 或DataAdapter ,然后再使用取得的DataReader 或DataAdapter 对象操作数据结果。

下面是ADO.Net这五个对象之间的关系图

![ADO.NET中的五个主要对象](https://images0.cnblogs.com/blog/446435/201310/29213216-d210477ace3449baa0bbb98d8fab12ad.jpg)