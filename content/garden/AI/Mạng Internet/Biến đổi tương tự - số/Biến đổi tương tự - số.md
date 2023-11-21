- Các bước:
1. Lấy mẫu
	- lựa chọn 1 tần số lấy mẫu (fLM >= fmax với fmax)
	- fLM lần xung lấy mẫu trong 1 đơn vị thời gian (1delta)
	- fLM = 8Khz <=> 1 delta lấy mẫu 8000 lần
	- rời rạc về mặt thời gina để giảm lượng thông tin phải truyền đi
2. Lượng tử hóa (Quantization)
	- rời rạc hóa về mặt biên độ
	- sinh ra tạp âm lượng tử hóa do có sai số khi lượng tử, tạp âm lượng tử hóa gọi là (quantization noise). Sai số lớn nhất là A/8
	- trên thực tế thì sẽ có 255 dải chia làm 256 mức biên độ khác nhau nên sai số hoàn toàn chấp nhận được
3. Mã hóa coding
	- chuyển xung theo dòng nhị phân
	- 5 mức --> 3bit 
	- vd: 
		- 8000.3 =24000bit/s
		- thực tế 8000.8 = 64kbit/s
