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

 

