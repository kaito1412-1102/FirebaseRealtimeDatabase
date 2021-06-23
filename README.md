# FirebaseRealtimeDatabase
document: https://firebase.google.com/docs/database/android/start

build.gradle:(Module app)    
android {
  .....
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    apply plugin: 'com.android.application'
    apply plugin: 'com.google.gms.google-services'
}
library:
    implementation platform('com.google.firebase:firebase-bom:28.1.0')
    implementation 'com.google.firebase:firebase-database'
    
build.gradle:    
    buildscript {
    repositories {
        google()
        jcenter()
    }
    dependencies {
        classpath "com.android.tools.build:gradle:4.0.1"
        classpath 'com.google.gms:google-services:4.3.8'
    }
}

![Capture](https://user-images.githubusercontent.com/68551096/123069839-6525c600-d43d-11eb-9b8d-9891e5437462.PNG)
Khai báo trong MainActivity:

    FirebaseDatabase database = FirebaseDatabase.getInstance();
    DatabaseReference dataBaseAuthor = database.getReference("table_author");
    
- Khi muốn thêm 1 Author vào database

    String id = dataBaseAuthor.push().getKey();
    Author author = new Author(id, authorName);
    dataBaseAuthor.child(id).setValue(author).addOnSuccessListener(new OnSuccessListener<Void>() {
      @Override
      public void onSuccess(Void aVoid) {
        adapter.addAuthor(author);
      }
    });
    
![Capture1](https://user-images.githubusercontent.com/68551096/123071931-488a8d80-d43f-11eb-8052-03b46ddc780c.PNG)
- Khi muốn update Author => ta cần id của author đó

  DatabaseReference databaseReference = database.getReference("table_author").child(athorId);
  Author author = new Author(athorId, authorName);
  databaseReference.setValue(author);
  
Câu lệnh trên giúp ta update cả đối tượng author vào database tuy nhiên nếu bạn chỉ muốn cập nhật 1 thuộc tính mà k cần viết lại cả đối tượng thì ta làm như sau:
  DatabaseReference databaseReference = database.getReference("table_author").child(athorId);
  databaseReference.child("name").setValue(authorName);

- Khi ta muốn delete 1 Author:
   DatabaseReference databaseReference = database.getReference("table_author").child(id);
   databaseReference.removeValue();

- Khi ta muốn lấy dữ liệu từ databse:
  C1: Sử dụng addValueEventListener
  Ưu điểm: Lắng nghe sự thay đổi từ database. Khi database được thêm mới, chỉnh sửa hoặc xoá thì nó lập tức trả lại toàn bộ danh sách đã được cập nhật
  Nhược điểm: Tiêu tốn tài nguyên
  ![Capture2](https://user-images.githubusercontent.com/68551096/123074063-30b40900-d441-11eb-8866-73d51201f1f2.PNG)
  C2: Sử dụng addOnCompleteListener => Chỉ lấy dữ liệu 1 lần duy nhất
  ![Capture3](https://user-images.githubusercontent.com/68551096/123074360-6953e280-d441-11eb-9c06-f5fc09f98a71.PNG)

- Sắp xếp dữ liệu trả về: document: https://firebase.google.com/docs/database/android/lists-of-data#sort_data
  ![Capture4](https://user-images.githubusercontent.com/68551096/123074844-df584980-d441-11eb-93f6-cbd2f09aa191.PNG)
  
- Tìm kiếm và phân trang:
  ![Capture5](https://user-images.githubusercontent.com/68551096/123075414-6a394400-d442-11eb-85e0-187e74f81aa1.PNG)
  ![Capture6](https://user-images.githubusercontent.com/68551096/123075679-a8cefe80-d442-11eb-973b-5d9c716c16cd.PNG)


  
