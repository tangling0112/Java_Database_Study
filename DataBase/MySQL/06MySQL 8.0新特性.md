### 1 计算列

- ```SQL
  DROP TABLE IF EXISTS TEST;
  CREATE TABLE TEST(
  a INT,
  b INT,
  c INT GENERATED ALWAYS AS (a + b) VIRTUAL
  );
  INSERT INTO TEST(a,b)
  VALUES(50,15);
  
  SELECT *
  FROM TEST; 
  /*
  此时C就是我们的计算列其效果为,我们向表中插入数据时,可以认为表中只有a,b两个字段.而至于字段c,系统会自动根据我们的字段a,b来生产
  */
   
  ```

### 2 `NATUAL JOIN`与`USING`

### 3 窗口函数

### 4 公用表表达式