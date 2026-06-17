# BÀI 3: Đọc hiểu & Dò lỗi qua Prompt (Phát hiện Ảo giác AI - Hallucination)

## Giải thích nguyên lý sinh lỗi ảo giác của AI

Lỗi ảo giác (Hallucination) xảy ra khi AI tạo ra thông tin nghe có vẻ hợp lý nhưng thực tế không tồn tại. Trong trường hợp này, prompt đã giả định rằng thư viện Apache Commons Lang có phương thức `StringUtils.hasSpecialCharacters()`. Do AI thường cố gắng hoàn thành yêu cầu dựa trên ngữ cảnh được cung cấp, nó có thể suy đoán và sinh ra lời gọi đến một phương thức không có thật thay vì xác minh sự tồn tại của phương thức đó.

Kết quả là mã nguồn được tạo ra có thể trông hợp lý nhưng sẽ không biên dịch được. Đây là lý do khi làm việc với AI, lập trình viên cần kiểm chứng lại API, thư viện và tài liệu chính thức thay vì tin tưởng hoàn toàn vào mã nguồn do AI sinh ra.

## Prompt tối ưu mới

```text id="76nq9m"
Hãy viết một hàm Java kiểm tra xem một chuỗi có chứa ký tự đặc biệt hay không. Chỉ sử dụng các API chính thức của Java Standard Library hoặc các thư viện phổ biến có tài liệu chính thức rõ ràng. Không được tự tạo hoặc sử dụng các phương thức chưa được xác thực.

Sau khi viết mã nguồn, hãy giải thích phương thức hoặc lớp được sử dụng và nêu cơ sở chứng minh rằng API đó thực sự tồn tại trong tài liệu chính thức của Java hoặc thư viện tương ứng.
```

## Mã nguồn Java do AI sinh ra

```java id="3c2l9l"
import java.util.regex.Pattern;

public class SpecialCharacterChecker {

    private static final Pattern SPECIAL_CHARACTER_PATTERN =
            Pattern.compile("[^a-zA-Z0-9 ]");

    public static boolean hasSpecialCharacters(String text) {

        if (text == null) {
            return false;
        }

        return SPECIAL_CHARACTER_PATTERN.matcher(text).find();
    }

    public static void main(String[] args) {
        System.out.println(hasSpecialCharacters("Hello World"));   // false
        System.out.println(hasSpecialCharacters("Hello@World"));   // true
        System.out.println(hasSpecialCharacters("Java#2025"));     // true
    }
}
```

### Giải thích cơ sở

Mã nguồn sử dụng lớp `java.util.regex.Pattern` và phương thức `matcher().find()` thuộc Java Standard Library để kiểm tra sự xuất hiện của ký tự đặc biệt theo biểu thức chính quy. Đây là các API chính thức của Java và được tài liệu hóa trong Java SE Documentation. Vì vậy, giải pháp này đáng tin cậy hơn việc sử dụng một phương thức không được xác thực như `StringUtils.hasSpecialCharacters()`.
