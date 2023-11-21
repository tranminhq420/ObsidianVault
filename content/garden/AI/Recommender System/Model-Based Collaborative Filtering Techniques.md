- Việc thiết kế và phát triển các models cho phép hệ thống học cách nhận biết các patterns phức tạp dựa vào dữ liệu được dùng để train, từ đó có thể tạo các tiên đoán thông minh cho các task collaborative filtering với dữ liệu thử hoặc thật
- Models based CF giải quyết được các vấn đề chính của memory based CF algo
- #### Các thuật toán: 
	-  ==Các thuật toán Bayesian Belief Net CF:==
		- ==**Định nghĩa:**== Bayesian Belief net là một directed graph với triplet <N,A,Θ>, trong đố mỗi node n thuộc N đại diện cho 1 tham số,  arc a (thuộc A) giữa các node đại diện cho các association xét theo mặt khả năng giữa các tham số,  Θ là một table conditional probabilty định lượng độ phụ thuộc của các node con vào các nốt cha
		- Các thuật toán BN:
			- Simple Bayesian CF Algorithm
				- sử dụng thuật toán naive Bayes để tạo các dự đoán cho các task CF
				- giả thiết là tất cả các feat của là độc lập với nhau, có thể tính được khả năng của một class với các features nhất định có thể được tính toán
				- class có khả năng cao nhất sẽ được cho là class được tiên đoán
				- với các dữ liệu không hoàn chỉnh thì tính toán khả năng và phân loại sẽ được tính toán bằng các data đã thu thập được
				- hầu hết chỉ sử dụng được với binary class data
			- Các thuật toán NB-ELR và TAN-ELR CF:
				- Vì hạn chế của thuật toán Bayes đơn giản nên tạo ra thuật toán BN với khả năng deal với dữ liệu không đầy đủ
				- ELR là thuật toán gradient ascent, tăng giảm độ dốc
				- cả hai thuật toán đều được chứng minh là có tính phân loại với độ chính xác cao với cả dữ liệu hoàn chỉnh và không hoàn chỉnh
				- Cần nhiều thời gian hơn để train models
			- Các thuật toán Bayesian CF khác:
				- Bayesian belief nets with decision trees at each node:
					- model có decision tree tại mỗi node của BN, mỗi node thể hiện 1 item trong domain và states của mỗi node thì thể hiện các ratings của các items
				- Baseline Bayesian model:
					- model sử dụng BN với không arc
					- rec items dựa trên popularity
	-  Các thuật toán co cụm CF:
		- Một cluster là một collection mà trong đó các data giống nhau và khác với các data ở trong cluster khác
		- thước đo sự tương đồng ở đây được đo bằng các đơn vị như là khoảng cách Minkowski hoặc tự tương quan Pearson
		- có công thức
			 ![[Pasted image 20231101214146.png]]
			 trong đó n là số chiều của vật và xi yi là các giá trị của chiều thứ i của vật X và Y,
			 q là một số nguyên dương.
			Khi q = 1 thì d là __khoảng cách Manhattan__ và khi q = 2 thì d là __khoảng cách Euclide__ 
		-  các phương pháp co cụm có thể chia làm ba cách:
			- partitioning methods:
					- một phương pháp partition hay được sử dụng là [[k-means method]] 
					
			 - density-based methods:
					- tìm các cụm objects dày đặc mà được chia cắt bằng các vùng thưa, tượng trưng cho các loại thông tin nhiễu
					- các phương pháp tiêu biểu gồm có DBSCAN và OPTICS
					
			 - hierarchical methods:
				 - tiêu biểu có BIRCH 
				 - phân rã dữ liệu ra thành dạng tập hợp dữ liệu phân cấp dựa vào các tiêu chí 
				 - trong hầu hết các tình huống thì co cụm chỉ là bước trung gian và các cluster được tính toán ra được sử dụng để phân tích thêm hoặc phân loại hoặc các tasks khác
				 - model hỗn hợp linh hoạt(FMM) kế thừa các thuật toán co cụm cho CF bằng cách cluster cả users và các items cùng lúc -> cho phép mỗi user và item trong nhiều cluster cùng lúc và tạo model cho các cluster users và items riêng.
				FMM có độ chính xác cao hơn thuật toán dựa trên độ tương quan Pearson và aspect model
		- các models theo cụm có scalability tốt hơn các phương pháp lọc bình thường bởi vì nó tạo các tiên đoán trong các cụm nhỏ thay vì trong cả cái customer base. Chất lượng gợi ý thấp nhưng có thể cải thiện nhưng chi phí sẽ tăng ngang với mem-based CF
	-  Các thuật toán regression-base:
		- đối với các thuật toán CF mem-based, hai vector đánh giá có thể có có khoảng cách euclide lớn nhưng lại tương đồng với nhau khi dùng vector côsin hoặc đánh giá tương quan Pearson -> mem-based CF không phù hợp và cần giải pháp tốt hơn
		- thường được dùng để tiên đoán với sử dụng giá trị số.
			- sử dụng ratings xấp xỉ để tiên đoán dựa trên model regression
			- Cho tập X là tập các trị số thể hiện user preference  đối với các item khác nhau, ta có:
			![[Pasted image 20231103120829.png]]
			trong đó Λ là một ma trận n x k
			N là tập các tham số ngẫu nhiên tượng trưng cho các dữ liệu nhiễu trong lựa chọn của người dùng
			Y là ma trận n x m với Yij là rating của người dùng i lên item j
			X là ma trận k x m với mỗi cột là một giá trị xấp xỉ của một tham số bất kì K (user's rating trong không gian đánh giá k chiều)
			-> Thường thì ma trận Y rất thưa -> Canny đưa ra sparse factor analysis để sửa điều này, thay thế các phần tử thiếu với giá trị vote mặc định và sử dụng model regression làm giá trị khởi tạo cho các vòng lặp Expectation Maximization
	- Các thuật toán MDP: 
		- Shani coi việc gợi ý là vấn đề tối ưu hóa chuỗi thay vì là vấn đề tiên đoán và sử dụng model quy trình quyết định Markov MDP cho các hệ recommender
		- một model MDP là một model cho các vấn đề quyết định trình tự ngẫu nhiên
		- thường được sử dụng cho các ứng dụng mà một nguyên tố làm ảnh hưởng đến môi trường xung quanh qua các hành động của nó
		- một MDP được định nghĩa qua <S,A, R, Pr> trong đó:
			- S là tập các trạng thái
			- A là tập các hành đồng
			- R là các hàm phần thưởng giá trị thực cho mỗi cặp hành động trạng thái
			- Pr là xác suất chuyển đổi giữa mỗi cặp trạng thái với mỗi hành hành động
		- model MDP CF hiệu quả hơn nhiều so với model chuỗi Markov đơn giản(MDP nhưng không có tập A)
	- Các models Latent Semantic CF:
		-  kĩ thuật dựa trên một kĩ thuật model xác suất, sử dụng các tham số class ẩn trong hỗn hợp model để tìm ra các cộng đồng mà users thuộc về và các mối quan tâm điển hình.
		-  nó phân rã các mối quan tâm của user bằng cách overlap các cộng đồng của user
		-  hơn mem-based ở chỗ n chính xác hơn và scale tốt hơn
		- điển hình có aspect model và multinominal model
		- Các kĩ thuật model-based khác:
			- Cohen đã tạo ra một cách tiếp cận là học trình tự hai bước CF.
				- hoạt động tốt hơn thuật toán nearest neighbor và linear regression
			- Các thuật toán dựa trên luật tương quan
				- thường được dùng cho các gợi ý top-N hơn là các gợi ý mang tính tiên đoán
			- maximum entropy
			- dependency network
			- Decision tree CF models
			- Matrix factorization based CF
	- ==Các kĩ thuật Hybrid CF:==
		- các hệ CF hybrid kết hợp CF với các kĩ thuật  gợi ý khác, thường là content-based để tạo các tiên đoán hoặc gợi ý
		- Các thuật toán Hybrid CF: 
			- hệ hybrid tích hợp CF và các đặc trưng của content based:
				- sử dụng naive Bayes làm phân loại content, sau đó điền các dữ liệu trống trong ma trận rating bằng các tiên đoán của phần tử tiên đoán nội dung để tạo ra một ma trận rating giả, trong đó các dữ liệu quan sát được được dữ nguyên và các dữ liệu trống được thay thế bằng các dữ liệu tiên đoán. Sau đó ra các tiên đoán sử dụng các thuật toán CF dựa trên tương quan  có trọng số Pearson. Item càng được nhiều người dùng rate thì có trọng số càng cao.
				- không có vấn đề cold start và không có vấn đề với các dữ liệu thưa. Perfomance hơn cả thuần mem-based hoặc content-based
			- Hệ Hybrid tích hợp CF và các hệ gợi ý khác:
				- một hệ gợi ý có trọng số kết hợp các kĩ thuật gợi ý khác nhau dựa trên các trọng số, được tính sử dụng kết quả của tất cả các kĩ nghệ gợi ý khác trong hệ thống.
				- sự kết hợp này có thể là tuyến tính, trọng số có thể thay đổi, và có thể sử dụng majority voting có trọng số.
				- liên tục đổi các kĩ thuật gợi ý dựa trên các tiệu chí
			- Hệ Hybrid dựa trên kết hợp các thuật toán CF:
				- hai class chính của các cách tiếp cận với hệ gợi ý CF, mem-based và model-based, có thể được kết hợp để hình thành các cách tiếp cận CF hybrid khác
				- Các thuật toán:
					- Probabilistic memory-based collaborative filtering (PMCF)
						- kết hợp mem-based và model-based
						- sử dụng kết hợp hỗn hợp dựa trên base là một tập hợp các profile người dùng đã được lưu và sử dụng các xác suất hậu nghiệm (posterior distribution) để thực hiện các tiên đoán
					- Personality diagnosis (PD)
						- có các lợi ích từ cả mem-based và model-based
						- tạo ra một người dùng giả định bằng cách chọn một người dùng random và cho các dữ liệu nhiễu Gaussian vào ratings của người ta.
						- dựa vào các ratings đã có, tính toán khả năng người dùng này sẽ có kiểu tính cách với người dùng khác và các xác suất một người dùng sẽ thích cùng một item.
						- có thể coi là một phương thức co cụm với một người dùng mỗi cụm
						- tiên đoán tốt hơn tương quan Pearson và các thuật toán vector CF dựa trên sự tương đồng và hai thuật toán model-based khác: Bayesian clustering và Bayesian Network