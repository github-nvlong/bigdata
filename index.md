# WEEK 1
## Tìm hiểu Apache Spark
Apache Spark là một open source cluster computing framework được phát triển sơ khởi vào năm 2009 bởi AMPLab tại đại học California, Berkeley. Sau này, Spark đã được trao cho Apache Software Foundation vào năm 2013 và được phát triển cho đến nay.
Khi ta có một tác vụ nào đó qúa lớn mà không thể xử lý trên một laptop hay một server, Spark cho phép ta phân chia tác vụ này thành những phần dễ quản lý hơn. Sau đó, Spark sẽ chạy các tác vụ này trong bộ nhớ, trên các cluster của nhiều server khác nhau để khai thác tốc độ truy xuất nhanh từ RAM. Spark sử dụng API Resilient Distributed Dataset (RDD) để xử lý dữ liệu.
Apache Spark gồm có 5 thành phần chính: Spark Core, Spark Streaming, Spark SQL, MLlib và GraphX
1.	Spark Core là nền tảng cho các thành phần còn lại và các thành phần này muốn khởi chạy được thì đều phải thông qua Spark Core
2.	Spark SQL cung cấp một kiểu data abstraction mới (SchemaRDD) nhằm hỗ trợ cho cả kiểu dữ liệu có cấu trúc (structured data) và dữ liệu nửa cấu trúc (semi-structured data).
3.	Spark Streaming được sử dụng để thực hiện việc phân tích stream bằng việc coi stream là các mini-batches và thực hiệc kỹ thuật RDD transformation đối với các dữ liệu mini-batches này.
4.	MLlib (Machine Learning Library): MLlib là một nền tảng học máy phân tán bên trên Spark do kiến trúc phân tán dựa trên bộ nhớ.
5.	GrapX: Grapx là nền tảng xử lý đồ thị dựa trên Spark. Nó cung cấp các Api để diễn tả các tính toán trong đồ thị bằng cách sử dụng Pregel Api.
Những điểm sáng giá ngoài tốc độ tính toán nhanh
- Sự đơn giản: Một trong những chỉ trích thường gặp ở Hadoop đó là sự phức tạp trong qúa trình phát triển, mặc dù đây là một trong những phương pháp tính toán đơn gỉan và hiệu qủa gíup tăng tốc độ xử lý của hệ thống. Thay vì đòi hỏi người dùng phải hiểu rạch ròi về MapReduce và lập trình Java, Spark sinh ra để gíup mọi người tiếp cận với công nghệ tính toán song song dễ dàng hơn rất nhiều. Người dùng chỉ cần một vài kiến thức cơ bản về database cộng với lập trình Python hay Scala là có thể sử dụng được.
- Độc lập với các nhà cung cấp dịch vụ Hadoop: Hầu hết các nhà cung cấp dịch vụ Hadoop đều hỗ trợ Spark. Điều này có nghĩa Spark không phụ thuộc vào các nhà cung cấp này. Nếu bạn muốn thay đổi nhà cung cấp dịch vụ, ta chỉ cần đem hệ thống Spark qua nhà cung cấp mới mà không lo ngại việc mất mát thông tin.
## Mapreduce
### Tổng quan
- MapReduce là mô hình được thiết kế độc quyền bởi Google, nó có khả năng lập trình xử lý các tập dữ liệu lớn song song và phân tán thuật toán trên 1 cụm máy tính. MapReduce trở thành một trong những thành ngữ tổng quát hóa trong thời gian gần đây. 
- MapReduce sẽ  bao gồm những thủ tục sau: thủ tục 1 Map() và 1 Reduce(). Thủ tục Map() bao gồm lọc (filter) và phân loại (sort) trên dữ liệu khi thủ tục khi thủ tục Reduce() thực hiện quá trình tổng hợp dữ liệu. Đây là mô hình dựa vào các khái niệm biển đối của bản đồ và reduce những chức năng lập trình theo hướng chức năng. Thư viện của thủ tục Map() và Reduce() sẽ được viết bằng nhiều loại ngôn ngữ khác nhau. Thủ tục được cài đặt miễn phí và được sử dụng phổ biến nhất là là Apache Hadoop.
  - *Hàm Map()*: có nhiệm vụ nhận Input cho các cặp giá trị/  khóa và output chính là tập những cặp giá trị/khóa trung gian. Sau đó, chỉ cần ghi xuống đĩa cứng và tiến hành thông báo cho các hàm Reduce() để trực tiếp nhận dữ liệu. 
  - *Hàm Reduce()*: có nhiệm vụ tiếp nhận từ khóa trung gian và những giá trị tương ứng với lượng từ khóa đó. Sau đó, tiến hành ghép chúng lại để có thể tạo thành một tập khóa khác nhau. Các cặp khóa/giá trị này thường sẽ thông qua một con trỏ vị trí để đưa vào các hàm reduce. Quá trình này sẽ giúp cho lập trình viên quản lý dễ dàng hơn một lượng danh sách cũng như  phân bổ giá trị sao cho  phù hợp nhất với bộ nhớ hệ thống. 
  - Ở giữa Map và Reduce thì còn 1 bước trung gian đó chính là *Shuffle*. Sau khi Map hoàn thành  xong công việc của mình thì Shuffle sẽ làm nhiệm vụ chính là thu thập cũng như tổng hợp từ khóa/giá trị trung gian đã được map sinh ra trước đó rồi chuyển qua cho Reduce tiếp tục xử lý.
 ### Nguyên tắc hoạt động 
Mapreduce hoạt động dựa vào nguyên tắc chính là “Chia để trị”, như sau:
- Phân chia các dữ liệu cần xử lý thành nhiều phần nhỏ trước khi thực hiện. 
- Xử lý các vấn đề nhỏ theo phương thức song song trên các máy tính rồi phântán hoạt động theo hướng độc lập.
- Tiến hành tổng hợp những kết quả thu được để đề ra được kết quả sau cùng. 
### Các bước hoạt động của MapReduce
1. Tiến hành chuẩn bị các dữ liệu đầu vào để cho Map() có thể xử lý.
2. Lập trình viên thực thi các mã Map() để xử  lý. 
3. Tiến hành trộn lẫn các dữ liệu được xuất ra bởi Map() vào trong Reduce Processor
4. Tiến hành thực thi tiếp mã Reduce() để có thể xử lý tiếp các dữ liệu cần thiết.  
5. Thực hiện tạo các dữ liệu xuất ra cuối cùng.
Mapreduce là một mô hình lập trình, thực hiện quá tình xử lý tập dữ liệu lớn. Mapreduce gồm 2 pha : map và reduce.
Hàm Map : Các xử lý một cặp (key, value) để sinh ra một cặp (keyI, valueI) - key và value trung gian. Dữ liệu này input vào hàm Reduce.
Hàm Reduce : Tiếp nhận các (keyI, valueI) và trộn các cặp (keyI, valueI) trung gian , lấy ra các valueI có cùng key
### Ý tưởng
- Chia vấn đề cần xử lý thành các phần nhỏ để xử lý.
- Xử lý các phần nhỏ đó một cách song song và độc lập trên các máy tính phân tán.
- Tổng hợp các kết quả thu được để dưa ra kết quả cuối cùng.
Hoạt động của MapReduce có thể được tóm tắt như sau:
- Đọc dữ liệu đầu vào
- Xử lý dữ liệu đầu vào (thực hiện hàm map)
- Sắp xếp và trộn các kết quả thu được từ các máy tính phân tán thích hợp nhất.
- Tổng hợp các kết quả trung gian thu được ( thực hiện hàm reduce)
- Đưa ra kết quả cuối cùng.
# WEEK 2
## Spark Properties
Thuộc tính Spark kiểm soát hầu hết các cài đặt ứng dụng và được cấu hình riêng cho từng ứng dụng. Các thuộc tính này có thể được đặt trực tiếp trên SparkConf được chuyển tới SparkContext của bạn. SparkConf cho phép bạn định cấu hình một số thuộc tính phổ biến (ví dụ: URL chính và tên ứng dụng), cũng như các cặp khóa-giá trị tùy ý thông qua phương thức set ().
Khi nói đến thuộc tính Spark thì phải đề cập những vấn đề dưới đây
- Dynamically Loading Spark Properties (Tải động các thuộc tính Spark)
- Viewing Spark Properties (Xem thuộc tính Spark)
- Available Properties (Thuộc tính có sẵn)
- Environment Variables (Các biến môi trường)
- Configuring Logging (Định cấu hình ghi nhật ký)
- Overriding configuration directory (Ghi đè thư mục cấu hình)
- Inheriting Hadoop Cluster Configuration (Kế thừa cấu hình cụm Hadoop)
- Custom Hadoop/Hive Configuration (Cấu hình Hadoop / Hive tùy chỉnh)
## Spark RDD (Resilient Distributed Datasets)
Resilient Distributed Datasets (RDD) là một cấu trúc dữ liệu cơ bản của Spark. Nó là một tập hợp các đối tượng được phân phối bất biến. Mỗi tập dữ liệu trong RDD được chia thành các phân vùng logic, có thể được tính toán trên các nút khác nhau của cụm. RDD có thể chứa bất kỳ loại đối tượng Python, Java hoặc Scala nào, bao gồm các lớp do người dùng định nghĩa.
Về mặt hình thức, RDD là một tập hợp các bản ghi được phân vùng, chỉ đọc. RDD có thể được tạo thông qua các hoạt động xác định trên dữ liệu trên bộ lưu trữ ổn định hoặc các RDD khác. RDD là một tập hợp các phần tử chịu được lỗi có thể hoạt động song song.
Có hai cách để tạo RDDs:
- Tạo từ một tập hợp dữ liệu có sẵn trong ngôn ngữ sử dụng như Java, Python, Scala.
- Lấy từ dataset hệ thống lưu trữ bên ngoài như HDFS, Hbase hoặc các cơ sở dữ liệu quan hệ.
Spark sử dụng khái niệm RDD để đạt được các hoạt động MapReduce nhanh hơn và hiệu quả hơn. Trước tiên, chúng ta hãy thảo luận về cách các hoạt động MapReduce diễn ra và tại sao chúng không hiệu quả như vậy.
### Chia sẻ dữ liệu chậm trong MapReduce
MapReduce được sử dụng rộng rãi để xử lý và tạo các tập dữ liệu lớn với một thuật toán phân tán, song song trên một cụm. Nó cho phép người dùng viết các phép tính song song, sử dụng một tập hợp các toán tử cấp cao, mà không phải lo lắng về việc phân phối công việc và khả năng chịu lỗi.
Thật không may, trong hầu hết các khuôn khổ hiện tại, cách duy nhất để sử dụng lại dữ liệu giữa các lần tính toán (Ví dụ: giữa hai công việc MapReduce) là ghi nó vào hệ thống lưu trữ ổn định bên ngoài (Ví dụ - HDFS). Mặc dù khung công tác này cung cấp nhiều thông tin tóm tắt để truy cập tài nguyên tính toán của một cụm, người dùng vẫn muốn nhiều hơn thế.
Cả hai ứng dụng Lặp lại và Tương tác đều yêu cầu chia sẻ dữ liệu nhanh hơn trên các công việc song song. Chia sẻ dữ liệu chậm trong MapReduce do sao chép, tuần tự hóa và IO đĩa. Về hệ thống lưu trữ, hầu hết các ứng dụng Hadoop, chúng dành hơn 90% thời gian để thực hiện các thao tác đọc-ghi HDFS.
### Hoạt động lặp lại trên MapReduce
Sử dụng lại các kết quả trung gian qua nhiều phép tính trong các ứng dụng nhiều giai đoạn. Hình minh họa sau giải thích cách hoạt động của khung hiện tại trong khi thực hiện các hoạt động lặp lại trên MapReduce. Điều này phát sinh chi phí đáng kể do sao chép dữ liệu, I / O đĩa và tuần tự hóa, khiến hệ thống chậm.
![1](https://user-images.githubusercontent.com/58651463/106392708-a2a19c80-6425-11eb-8e66-1d9236448574.png)
### Hoạt động tương tác trên MapReduce
Người dùng chạy các truy vấn đặc biệt trên cùng một tập con dữ liệu. Mỗi truy vấn sẽ thực hiện I / O đĩa trên bộ nhớ ổn định, có thể chi phối thời gian thực thi ứng dụng.
Hình minh họa sau giải thích cách hoạt động của khung hiện tại khi thực hiện các truy vấn tương tác trên MapReduce.
![2](https://user-images.githubusercontent.com/58651463/106392715-aa614100-6425-11eb-89b7-1ad66bb53fbf.png)
### Chia sẻ dữ liệu bằng Spark RDD
Chia sẻ dữ liệu chậm trong MapReduce do sao chép, tuần tự hóa và IO đĩa. Hầu hết các ứng dụng Hadoop, chúng dành hơn 90% thời gian để thực hiện các thao tác đọc-ghi HDFS.
Nhận thức được vấn đề này, các nhà nghiên cứu đã phát triển một framework chuyên biệt có tên là Apache Spark. Ý tưởng chính của tia lửa là Tập dữ liệu phân tán có khả năng phục hồi (RDD); nó hỗ trợ tính toán xử lý trong bộ nhớ. Điều này có nghĩa là, nó lưu trữ trạng thái bộ nhớ như một đối tượng trên các công việc và đối tượng có thể chia sẻ giữa các công việc đó. Chia sẻ dữ liệu trong bộ nhớ nhanh hơn mạng và Đĩa từ 10 đến 100 lần.
### Thực thi trên Spark RDD
Để khắc phục được vấn đề về MapRedure, các nhà nghiên cứu đã phát triển một framework chuyên biệt gọi là Apache Spark. Ý tưởng chính của Spark là Resilient Distributed Datasets (RDD); nó hỗ trợ tính toán xử lý trong bộ nhớ. Điều này có nghĩa, nó lưu trữ trạng thái của bộ nhớ dưới dạng một đối tượng trên các công việc và đối tượng có thể chia sẻ giữa các công việc đó. Việc xử lý dữ liệu trong bộ nhớ nhanh hơn 10 đến 100 lần so với network và disk.

- Iterative Operation trên Spark RDD:
- 
![3](https://user-images.githubusercontent.com/58651463/106392716-aaf9d780-6425-11eb-83f3-b7053a9a09db.png)

- Interactive Operations trên Spark RDD:
- 
 ![4](https://user-images.githubusercontent.com/58651463/106392718-ab926e00-6425-11eb-8da0-ec667d4638f8.png)
 
### Các loại RDD
![5](https://user-images.githubusercontent.com/58651463/106392719-ac2b0480-6425-11eb-852a-90c523b8368b.png)

- Các RDD biểu diễn một tập hợp cố định, đã được phân vùng các record để có thể xử lý song song.
- Các record trong RDD có thể là đối tượng Java, Scale hay Python tùy lập trình viên chọn. Không giống như DataFrame, mỗi record của DataFrame phải là một dòng có cấu trúc chứa các field đã được định nghĩa sẵn.
- RDD đã từng là API chính được sử dụng trong series Spark 1.x và vẫn có thể sử dụng trong version 2.X nhưng không còn được dùng thường xuyên nữa.
- RDD API có thể được sử dụng trong Python, Scala hay Java:
  - Scala và Java: Perfomance tương đương trên hầu hết mọi phần. (Chi phí lớn nhất là khi xử lý các raw object)
  - Python: Mất một lượng performance, chủ yếu là cho việc serialization giữa tiến trình Python và JVM
## DATAFRAME - KHUNG DỮ LIỆU ĐA NĂNG
### Định nghĩa
Trong Spark, DataFrame là một tập hợp dữ liệu phân tán được tổ chức thành các cột được đặt tên. Về mặt khái niệm, nó tương đương với một bảng trong cơ sở dữ liệu quan hệ hoặc một khung dữ liệu trong R / Python, nhưng với các tối ưu hóa phong phú hơn. DataFrames có thể được xây dựng từ nhiều nguồn như: tệp dữ liệu có cấu trúc, bảng trong Hive, cơ sở dữ liệu (SQL) hoặc RDD hiện có.
    Ví dụ minh họa với Spark SQL:
    
![6](https://user-images.githubusercontent.com/58651463/106392720-acc39b00-6425-11eb-88e7-205e498d864d.png)

Tạo một DataFrame về nhân viên có Tên của nhân viên dưới dạng kiểu dữ liệu chuỗi, ID nhân viên là kiểu dữ liệu chuỗi, Số điện thoại của nhân viên dưới dạng kiểu dữ liệu số nguyên, Địa chỉ nhân viên dưới dạng chuỗi kiểu dữ liệu, Mức lương của nhân viên dưới dạng kiểu dữ liệu nổi. Dữ liệu của từng nhân viên được lưu theo từng hàng như hình trên.
### DataFrames được thiết kế để đa chức năng
#### Nhiều ngôn ngữ lập trình
Đặc tính tốt nhất của DataFrames trong Spark là hỗ trợ nhiều ngôn ngữ, giúp các lập trình viên từ các nền tảng lập trình khác nhau sử dụng dễ dàng hơn. DataFrames trong Spark hỗ trợ R - Ngôn ngữ lập trình, Python, Scala và Java.
#### Nhiều nguồn dữ liệu
DataFrames trong Spark có thể hỗ trợ nhiều nguồn dữ liệu khác nhau.

![7](https://user-images.githubusercontent.com/58651463/106392724-b0efb880-6425-11eb-99f4-86c12f3b5fb6.png)

#### Xử lý dữ liệu có cấu trúc và bán cấu trúc
Yêu cầu cốt lõi mà DataFrames được giới thiệu là xử lý Dữ liệu lớn một cách dễ dàng. DataFrames trong Spark sử dụng định dạng bảng để lưu trữ dữ liệu theo cách linh hoạt cùng với lược đồ cho dữ liệu mà nó đang xử lý.
#### Slicing và Dicing dữ liệu
API DataFrame hỗ trợ Slicing và Dicing dữ liệu. Nó có thể thực hiện các thao tác như chọn và lọc theo hàng và cột. Dữ liệu thống kê luôn có xu hướng bị Thiếu giá trị, Vi phạm phạm vi và giá trị không liên quan. Người dùng có thể quản lý dữ liệu bị thiếu một cách rõ ràng bằng cách sử dụng DataFrames.
### Các tính năng của DataFrame trong Spark

![8](https://user-images.githubusercontent.com/58651463/106392725-b1884f00-6425-11eb-9f8a-c4bb27f10997.png)	 

DataFrame trong spark có bản chất là Bất biến. Giống như Tập dữ liệu được phân phối có khả năng phục hồi, dữ liệu có trong DataFrame không thể bị thay đổi.
Việc lười đánh giá là chìa khóa cho hiệu suất đáng chú ý do Spark mang lại. DataFrames trong Spark sẽ không hiển thị đầu ra trên màn hình trừ khi một thao tác hành động được kích hoạt.
Kỹ thuật Bộ nhớ phân tán được sử dụng để xử lý dữ liệu làm cho chúng có khả năng chịu lỗi.
Giống như Tập dữ liệu phân tán có khả năng phục hồi, DataFrames trong Spark mở rộng thuộc tính của mô hình bộ nhớ phân tán. Cách duy nhất để thay đổi hoặc sửa đổi dữ liệu trong DataFrame sẽ là áp dụng Chuyển đổi.
### Nguồn cho Spark Data Frame
![9](https://user-images.githubusercontent.com/58651463/106392727-b2b97c00-6425-11eb-9d8f-607380a8734b.png)  

Có rất nhiều cách để tạo DataFrame trong Spark như:
Dữ liệu có thể được tải vào thông qua CSV, JSON, XML, SQL, RDBMS và nhiều hơn nữa. Nó cũng có thể được tạo bằng cách sử dụng RDD hiện có và thông qua bất kỳ cơ sở dữ liệu nào khác, như Hive, HBase, Cassandra. Nó cũng có thể lấy dữ liệu từ HDFS hoặc hệ thống tệp cục bộ.
 
### Thí dụ DataFrame
Cài đặt Spark

![Khoitao](https://user-images.githubusercontent.com/58651463/116820716-9356fa00-aba0-11eb-973c-4b3715642130.png)

Khởi tạo dữ liệu

![Data](https://user-images.githubusercontent.com/58651463/116820768-dadd8600-aba0-11eb-9c92-c3ab1c7f5f3c.png)

Hiển thị dữ liệu

![show](https://user-images.githubusercontent.com/58651463/116820771-db761c80-aba0-11eb-9199-24776de13524.png)

Xem các kiểu thuộc tính của dữ liệu

![schema](https://user-images.githubusercontent.com/58651463/116820769-db761c80-aba0-11eb-8c73-502efd8a26b3.png)

In label, đếm dữ liệu, tổng label

![columes](https://user-images.githubusercontent.com/58651463/116820767-da44ef80-aba0-11eb-86ea-c1460bfb6b1b.png)

Chọn thuộc tính để xem

![select](https://user-images.githubusercontent.com/58651463/116820766-d913c280-aba0-11eb-8dd6-c40258c13c6d.png)

https://spark.apache.org/docs/latest/configuration.html

https://www.tutorialspoint.com/apache_spark/apache_spark_rdd.htm

https://laptrinh.vn/books/apache-spark/page/apache-spark-rdd

https://www.edureka.co/blog/dataframes-in-spark

Code https://colab.research.google.com/drive/19oLi-1z14q2PVnxxRjIxNyCOIuLC9MOo?usp=sharing




