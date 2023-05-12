<h2>CodeCommit</h2>

<h3> Định nghĩa </h3>

  - Dịch vụ được quản lý lưu trữ các kho Git private.

    + Khi chúng ta sử dụng CodeCommit, phía dưới CodeCommit là git và chúng ta có thể sử dụng Git Command để quản lý repository của chúng ta.
    + Quản lý: nó là dịch vụ quản lý. AWS quản lý giúp chúng ta về server và dung lượng của chúng, và respositories của chúng ta có thể tăng lên không giới hạn.

  Question: Tại sao không chỉ sử dụng Git ?

  Answer: Đương nhiên là có thể, và chúng ta có thể sử dụng một loại kho khác ngoài CodeCommit.

  - Lợi ích:

    + Dịch vụ được quản lý.
    + Tính sẵn sàng cao, mở rộng và khả năng chịu lỗi tốt.
    + Không giới hạn về kích thước.
    + Tích hợp với các dịch vụ khác của AWS: CodeBuild, CodePipeline, CodeDeploy, Lambda, SNS.
    + Hoạt động với các công cụ dựa trên Git.
  
<h3> Data Security </h3>

  - Khi chúng ta nghĩ đến security và lựa chọn CodeCommit:

    + Dữ liệu được mã hoá in transit và at rest: AWS KMS.
    + Dịch vụ quản lý của AWS an toàn.
    + Tính sẵn sàng cao và khả năng mở rộng tốt.


  - Kịch bản dữ liệu an toàn:
    
    + Chúng ta muốn team phát triển có full access vào CodeCommit, nhưng họ không được phép tạo và xoá repositories.
    + Sử dụng IAM để tạo nhóm phát triển với Policy phù hợp với quyền access vào CodeCommit. Sau đó tiến hành thêm user vào groups => Team phát triển sẽ có quyền đầy đủ với CodeCommit, ngoại trừ tạo và xoá repo.

  - Tips:
    + Repositories được mã hoá tự động at rest.
    + Repositories được mã hoá khi vận chuyển khi sử dụng HTTPS, SSH or cả 2 (có thể định cấu hình khi thiết lập).
    + 1 phần của bảo mật: chúng ta hãy nghĩ đến IAM groups: giới hạn quyền tới các user tương ứng.