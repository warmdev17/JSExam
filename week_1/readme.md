# Bài kiểm tra lập trình tuần 1

## Câu 1

- Cho đoạn code xử lí vòng lặp sau: (Đã ẩn đoạn code gốc)

- Yêu cầu: Nhận định "Đoạn code này sẽ in ra lần lượt các số 0, 1, 2" là đúng hay sai? Nếu sai, hãy giải thích nguyên nhân cốt lõi và viết lại đoạn code bằng kiến thức ES6 để chương trình in ra đúng dãy số 0, 1, 2.

- Trả lời: Nhận định bên trên là đúng.

- Khi dùng `let` khai báo biến lặp, với mỗi vòng lặp Javascript sẽ tạo 1 vùng nhớ riêng cho vòng lặp đó và nhờ cơ chế `closure` của `setTimeout`, `setTimeout` khi register sẽ tham chiếu đến vùng nhớ do vòng lặp tạo ra và cuối cùng sau khi for chạy xong, các `setTimeout` được register sẽ lần lượt in 0, 1, 2.
- Nhưng nếu dùng `var` khai báo biến lặp, với mỗi vòng lặp Javascript chỉ tạo 1 vùng nhớ duy nhất cho biến i xuyên suốt vòng for nên sau khi for kết thúc cả 3 `setTimeout` register tham chiếu đến vùng nhớ duy nhất nên in ra 3.

---

## Câu 2

- Cho mảng dữ liệu mô phỏng được lấy về từ cơ sở dữ liệu: (Đã ẩn mảng users gốc)

- Yêu cầu: Nhận định "Chỉ cần dùng duy nhất hàm .map() kết hợp với câu lệnh if là có thể trả về một mảng nguyên sạch chỉ chứa id của các user thỏa mãn điều kiện role là 'admin' và active là true" là đúng hay sai? Hãy viết một đoạn code tối ưu nhất áp dụng Arrow Function để giải quyết bài toán lấy ra mảng id trên.

- Trả lời: Không thể, vì map sẽ trả về mảng có số lượng phần tử luôn luôn bằng với mảng gốc.

- Code:

```javascript
const adminIds = users
  .filter((u) => u.role === "admin" && u.active)
  .map((u) => u.id);

// Thật ra muốn tối ưu chạy 1 vòng lặp duy nhất thì xài reduce ngon hơn:
// const adminIds = users.reduce((acc, u) => {
//   if (u.role === 'admin' && u.active) acc.push(u.id);
//   return acc;
// }, []);
```

---

## Câu 3

- Yêu cầu: Nhận định "Cú pháp Spread Operator (...) khi dùng để sao chép một object sẽ luôn tạo ra một bản sao độc lập hoàn toàn (deep copy), kể cả đối với các object hoặc mảng lồng nhau (nested properties)" là đúng hay sai?

- Trả lời: Sai bét. Spread operator thực chất nó chỉ copy nông (shallow copy) thôi. Tức là mấy cái thuộc tính ở level ngoài cùng thì nó tạo mới ok, nhưng nếu có mảng hay object lồng bên trong (nested properties) thì nó vẫn xài chung tham chiếu vùng nhớ với thằng gốc. Mình mà đụng vào sửa cái mảng lồng bên trong bản copy là bản gốc cũng tạch theo luôn.

- Thực hành: Cho `const state = { user: 'Admin', privileges: ['read', 'write'] }`. Hãy dùng cú pháp ES6 để tạo ra một object newState giữ nguyên các thông tin cũ, nhưng bổ sung thêm quyền 'delete' vào mảng privileges, đồng thời tuyệt đối không làm biến đổi (mutate) mảng privileges của object state ban đầu.

- Code:

```javascript
const state = { user: "Admin", privileges: ["read", "write"] };

const newState = {
  ...state,
  // Dùng thêm spread một lần nữa cho mảng privileges để tạo mảng mới, tránh vụ mutate
  privileges: [...state.privileges, "delete"],
};
```

---

## Câu 5

- Cho đoạn code định nghĩa hàm tạo thông báo đơn hàng như sau: (Đã ẩn code gốc)

- Yêu cầu 1: Nhận định "Khi gọi createOrderMessage("Hải", null, "Bàn phím cơ"), tham số status sẽ tự động nhận giá trị mặc định là "pending" do null mang ý nghĩa là không có dữ liệu" là đúng hay sai? Hãy giải thích ngắn gọn nguyên lý hoạt động.

- Trả lời: Sai nha. Nguyên lý của default parameter trong ES6 là nó chỉ tự động nhảy về giá trị mặc định khi mình truyền chuẩn chữ `undefined` hoặc lơ luôn không truyền tham số đó thôi. Thằng `null` thì bản thân nó đã được JS coi là một giá trị có chủ đích rồi, nên tham số status nó sẽ gán cứng luôn chữ `null` chứ không thèm lấy "pending" đâu.

- Yêu cầu 2 (Thực hành): Viết logic bên trong thân hàm createOrderMessage. Sử dụng Template Literals (``) để trả về chuỗi thông báo theo quy tắc sau: Nếu khách hàng có mua sản phẩm (có tham số truyền vào items), trả về: "Khách hàng [user] có đơn hàng [status] gồm các món: [item1, item2,...]". Nếu khách hàng không truyền sản phẩm nào, trả về: "Khách hàng [user] có đơn hàng [status] nhưng giỏ hàng trống.".

- Code:

```javascript
function createOrderMessage(user, status = "pending", ...items) {
  if (items.length > 0) {
    // Nếu mảng items có đồ thì dùng hàm join nối tụi nó lại bằng dấu phẩy
    return `Khách hàng ${user} có đơn hàng ${status} gồm các món: ${items.join(", ")}`;
  } else {
    // Trường hợp mảng rỗng
    return `Khách hàng ${user} có đơn hàng ${status} nhưng giỏ hàng trống.`;
  }
}
```
