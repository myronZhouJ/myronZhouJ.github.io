---
layout: post
title:  "Sqlite不同方式插入数据的效率对比"
date:   2015-12-03 18:42:44
categories: jekyll update
---
设备Iphone6 64G iOS8.4


第一种方式：要插入一批数据时，每次插入数据都先对sql语句编译(准备)，然后执行（多次编译(准备)，多次执行），代码如下：
    NSLog(@"----begin-----");
    [self beginTransactionService];
    for (NSInteger i =0;i<500000;i++) {
        NSString *sql = [NSString stringWithFormat:
        @"INSERT INTO Slangs ('Content', 'Type') VALUES ('testText', '0')"];
        sqlite3_exec(_database, [sql UTF8String], NULL, NULL,NULL);
    }
    [self commitTransactionService];
    NSLog(@"----end-----");


2015-12-04 10:48:18.889 ViewControllerTest[2825:1131623] ----begin-----

2015-12-04 10:48:28.652 ViewControllerTest[2825:1131623] ----end-----

50万条，耗时9.8s


第二种方式：要插入一批数据前，先对sql语句编译(准备)，在每次插入时通过值绑定，然后执行（一次编译(准备)，多次绑定，多次执行），代码如下：
<pre><code>
NSLog(@"----begin-----");
[self beginTransactionService];
NSString *sql = [NSString stringWithFormat:@"INSERT INTO Slangs ('Content', 'Type') VALUES (?,?)"];
sqlite3_stmt *statement;
sqlite3_prepare_v2(_database,[sql UTF8String], -1, &statement,NULL);
for (NSInteger i =0;i<500000;i++) {
    const char *content = "testText";
    int type = 0;
    sqlite3_bind_text(statement, 1, content, (int)strlen(content), SQLITE_TRANSIENT);
    sqlite3_bind_int(statement, 2,type);
    if (sqlite3_step(statement) != SQLITE_DONE){
        sqlite3_finalize(statement);
        sqlite3_close(_database);
    }
    sqlite3_reset(statement);
}
sqlite3_finalize(statement);
[self commitTransactionService];
NSLog(@"----end-----");
</code></pre>

2015-12-04 10:46:11.256 ViewControllerTest[2810:1130838] ----begin-----

2015-12-04 10:46:12.898 ViewControllerTest[2810:1130838] ----end-----

相同的50万条，耗时1.6s

所以，以后如果需要大量插入数据还是用第二种方式吧.