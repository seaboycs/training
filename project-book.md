# 图书馆借书系统

## 系统架构

	Java Spring MVC + MySQL + JSP + JQUERY

## 需求说明

#### 角色

	User|Admin

#### 资源权限

	Book:Add|Delete|Borrow|Return

#### 角色权限

	Admin:Book:Add|Delete|Borrow|Return
	
	User:Book:Borrow

#### 功能点

	Admin:Add-Book(s)
	
	Admin:Delete-Book(s)
	
	Query:Book(s)
	
	Borrow:Book(s)
	
	Return:Book(s)
	
	User:Login
	
	User:Logout
	
	Query:Message(s)
	
	System:Send:Message(s)

## 系统设计

#### 数据建模

###### 图书分类 - BookCategory

	id:int|name:String|parent:BookCategory|idPath:String|namePath:String
	
	create table book_category(
		id int(3) primary key auto_increment,
		name varchar(20) not null,
		parentId int(3) null,
		idPath varchar(100) null,
		namePath varchar(300) null
	);
	
###### 图书 - Book

	id:int|name:String|category:BookCategory|author|year|introduction:String|status:int
	
	create table book(
		id int(5) primary key auto_increment,
		name varchar(50) not null,
		categoryId	int(3) null,		
		author varchar(20) null,
		year int(4) null,
		introduction varchar(2048) null,
		status int(1) default 0
	);

###### 用户 - User

	id:int|corpId:String|name:String
	
	create table user(
		id int(5) primary key auto_increment,
		corpId varchar(6) not null,
		name varchar(20) not null
	);

###### 角色 - Role
	id:int|name:String
	
	create table user(
		id int(1) primary key auto_increment,
		name varchar(20) not null
	);
	
	insert into user values(1, "Admin"),(2, "User");
	
###### 资源 - Resource

	id:int|name:String
	
	create table resource(
		id int(2) primary key auto_increment,
		name varchar(20) not null
	);
	
	insert into resource values(1, "Book");

###### 权限 - Permission

	id:int|name:String
	
	create table permission(
		id int(2) primary key auto_increment,
		name varchar(20) not null
	);
	
	insert into permission values(1, "Add"), (2, "Delete"), (3, "Borrow"), (4, "Return");
	
###### 角色资源权限	- RoleResourcePermission

	roleId:int|resourceId:int|permissionId:int
	
	create table role_resource_permission(
		roleId int(1) not null,
		resourceId int(2) not null,
		permissionId int(2) not null
	);
	
###### 用户角色 - UserRole

	userId:int|roleId:int
	
	create table user_role(
		userId int(5) not null,
		roleId int(1) not null
	);
	
###### 借书 - Booking

	id:int|book:Book|user:User|status:int|borrowDate:Date|returnDate:Date|Comments:String	
	
	create table booking(
		id int(6) primary key auto_increment,
		bookId int(5) not null,
		userId int(5) not null,
		status int(1) default 0,
		borrowDate date not null,
		returnDate date not null,
		comments varchar(200) null
	);
	
###### 借书记录 - BookingRecord

	id:int|booking:Booking|user:User|date:Date|operator:int|Comments:String
	
	create table booking_record(
		id int(8) primary key auto_increment,
		bookingId int(6) not null,
		userId int(5) not null,
		date	date not null,
		operator int(1) not null,
		comments varchar(200) null
	);
	
###### 消息 - Message

	id:int|title:String|content:String|fromUser:User|toUser:User|date:Date|status:int
	
	create table message(
		id int(8) primary key auto_increment,
		title varchar(50) not null,
		content varchar(2048) not null,
		fromUserId int(5) not null,
		toUserId int(5) not null,
		date datetime not null,
		status int(1) default 0
	);
	
###### 错误 - Error

	code:String|message:String
		
#### 接口定义（API）

###### 图书分类 - BookCategory

+ Get category by parentId

		URL: http://booking.acxiom.com/categories
		Method: Get
		Parameters:
			parentId:int:!required
		
		Example 1:
			Request: http://booking.acxiom.com/categories
			Response: {
				errors: [],
				data: [
					{id:1, name: name:"Java Books", parentId:null, idPath:"1", namePath:"Java Books"},
					{id:2, name: name:"C++ Books", parentId:null, idPath:"2", namePath:"C++ Books"},
					{id:3, name: name:"Node Books", parentId:null, idPath:"3", namePath:"Node Books"}
				]
			}
			
		Example 2:
			Request: http://booking.acxiom.com/categories?parentId=1
			Response: {
				errors: [],
				data: [
					{id:4, name: name:"Java入门图书", parentId:1, idPath:"1-4", namePath:"Java Books/Java入门图书"},
					{id:5, name: name:"Java中介图书", parentId:1, idPath:"1-5", namePath:"Java Books/Java中介图书"},
					{id:6, name: name:"Java高阶图书", parentId:1, idPath:"1-6", namePath:"Java Books/Java高阶图书"}
				]
			} 

+ Add a category

		URL: http://booking.acxiom.com/categories
		Method: Post
		Request Body: {
			name: "category name",
			parentId : 4
		}
		Response: {
			errors : [],
			data : {
				id : 7,
				name: "category name",
				parentId: 1,
				idPath: "1-4-7",
				namePath: "Java Books/Java入门图书/category name"
			}
		}

+ Delete a category

		URL: http://booking.acxiom.com/categories/{categoryId}
		Method: Delete
		Example: 
			Request: http://booking.acxiom.com/categories/2
		Response: {
			errors: [],
			data: true
		}

+ Delete sub categories

		URL: http://booking.acxiom.com/categories/{categoryId}/sub
		Method: Delete
		
		Example: 
			Request: http://booking.acxiom.com/categories/1/sub
				
		Response: {
			errors: [],
			data: true
		}
		
+ Update a category

		URL: http://booking.acxiom.com/categories/{categoryId}
		Method: Put		
		Request Body: {
			name : "new category name"
		}
		Response: {
			errors: [],
			data: {
				id : 7,
				name: "new category name",
				parentId: 1,
				idPath: "1-4-7",
				namePath: "Java Books/Java入门图书/category name"
			}
		}

###### 图书 - Book

+ Add a book

		URL: http://booking.acxiom.com/books
		Method: Post
		Request Body: {
			name: "Java Head First",
			categoryId: 4,
			author: "Tommy Zhao",
			year:2016,
			introduction: "Java Head First Introduction"
		}
		Response: {
			errors: [],
			data: {
				id: 1,
				name: "Java Head First",
				categoryId: 4,
				author: "Tommy Zhao",
				year: 2016,
				introduction: "Java Head First Introduction",
				status: 0
			}
		}

+ Update a book

		URL: http://booking.acxiom.com/books/{bookId}
		Method: Put
		
		Example: http://booking.acxiom.com/books/1
		Request Body: {
			name: "Java Head First 2",
			categoryId: 4,
			author: "Tommy Zhao",
			year:2016,
			introduction: "Java Head First 2 Introduction"
		}
		Response: {
			errors: [],
			data: {
				id: 1,
				name: "Java Head First 2",
				categoryId: 4,
				author: "Tommy Zhao",
				year: 2016,
				introduction: "Java Head First 2 Introduction",
				status: 0
			}
		}

+ Delete a book
	
		URL: http://booking.acxiom.com/books/{bookId}
		Method: Delete
		
		Example: http://booking.acxiom.com/books/1
		Response: {
			errors: [],
			data: true
		}
		
+ Query books

		URL: http://booking.acxiom.com/books
		Method: Get
		Parameters
			name:String:!required
			categoryId:int:!required
			author:String:!required
			year:int:!required
			status:int:!required
		
		Response: {
			errors: [],
			data: [
				{id:1, name:"Java Head First", category:{id:4, name:"Java入门图书"}, author:"Tommy Zhao", year:2016, status:1},
				{id:2, name:"Java Head First 2", category:{id:4, name:"Java入门图书"}, author:"Tommy Zhao", year:2016, status:1},
				{id:3, name:"Java Head First 3", category:{id:4, name:"Java入门图书"}, author:"Tommy Zhao", year:2016, status:0}
			]
		}

+ Get book

		URL: http://booking.acxiom.com/books/{bookId}
		Method: Get
		
		Response: {
			errors: [],
			data: {
				id: 1,
				name: "Java Head First",
				category: {
					id: 4,
					name: "Java入门图书",
					idPath: "1-4",
					namePath: "Java Books/Java入门图书"
				},
				author: "Tommy Zhao",
				year: 2016,
				introduction: "Java Head First 2 Introduction",
				status: 0
			}
		}

###### 借书 - Booking
		
+ Borrow book

		URL: http://booking.acxiom.com/bookings
		Method: Post
		Request: {
			bookId: 1,
			comments: "borrow this book"
		}
		Response: {
			errors: [],
			data: {
				bookingId: 1,
				book: {
					id: 1,
					name: "Java Head First"
				},
				user: {
					id: 2,
					name: "Tommy"
				},
				status: 1,
				borrowDate: "2016-06-07",
				returnDate: "2016-07-07",
				comments: "borrow this book",
				records:[
					{id:1, user:{id:2, name:"Tommy"}, date:"2016-06-07", operator:1, comments:"borrow this book"}				
				]				
			}
		}

+ Return book
		
		URL: http://booking.acxiom.com/bookings
		Method: Put
		Request: {
			bookingId: 1,
			comments: "return this book"
		}
		Response: {
			errors: [],
			data: {
				bookingId: 1,
				book: {
					id: 1,
					name: "Java Head First"
				},
				user: {
					id: 2,
					name: "Tommy"
				},
				status: 0,
				borrowDate: "2016-06-07",
				returnDate: "2016-07-07",
				comments: "borrow this book",
				records:[
					{id:1, user:{id:2, name:"Tommy"}, date:"2016-06-07", operator:1, comments:"borrow this book"},
					{id:1, user:{id:1, name:"Admin"}, date:"2016-07-01", operator:2, comments:"return this book"}
				]
			}
		}
		
###### 消息 - Message

+ Get messages sent to the user
		
		URL: http://booking.acxiom.com/messages/to/{userId}
		Method: Get
		
		Request:
			http://booking.acxiom.com/messages/to/2
			
		Response: {
			errors: [],
			data: [
				{id: 2, title: "Hello World", content: "Hello World!", fromUser: {id:1, name:"System"}, date: "2016-01-07 12:16", status:0 },
				{id: 1, title: "Hello World 2", content: "Hello World 2!", fromUser: {id:1, name:"System"}, date: "2016-01-07 12:05", status:1 }
			]
		} 
	
+ Get messages sent by the user

		URL: http://booking.acxiom.com/messages/from/{userId}
		Method: Get
		
		Request:
			http://booking.acxiom.com/messages/from/2
			
		Response: {
			errors: [],
			data: [
				{id: 4, title: "Hello World Reply", content: "Hello World Reply!", toUser: {id:1, name:"System"}, date: "2016-01-07 12:05", status:0 },
				{id: 3, title: "Hello World 2 Reply", content: "Hello World 2 Reply!", toUser: {id:1, name:"System"}, date: "2016-01-07 12:05", status:0 }
			]
		} 
	 
+ Send message

		URL: http://booking.acxiom.com/messages
		Method: Post
		
		Request Body: {
			fromUserId: 1,
			toUserIds: [2, 3],
			title: "Title",
			content: "Content"
		}
		
		Response: {
			errors: [],
			data: true
		}

+ Delete messages

		URL: http://booking.acxiom.com/messages
		Method: Delete
		
		Request Body: {
			ids: [1, 2]
		}
		
		Response: {
			errors: [],
			data: true
		}
				
#### 服务定义 - Interface

###### 图书分类 - BookCategoryInterface

	public interface BookCategoryInterface {
		
		public List<BookCategory> findCategories(int parentId) throws Exception;
		
		public BookCategory addCategory(BookCategory category) throws Exception;
		
		public BookCategory updateCategoryName(int categoryId, String categoryName) throws Exception;
		
		public boolean deleteCategory(int categoryId) throws Exception;
		
		public boolean deleteSubCategories(int categoryId) throws Exception;
	}

###### 图书 - BookInterface

	public interface BookInterface {
	
		public List<Book> findBooks(Book book) throws Exception;
		
		public Book getBook(int bookId) throws Exception;
		
		public Book addBook(Book book) throws Exception;
		
		public Book updateBook(Book book) throws Exception;
		
		public boolean deleteBook(int bookId) throws Exception;
	}
	
###### 借书 - BookingInterface

	public interface BookingInterface {
	
		public List<Booking> findActiveBookings(int bookId) throws Exception;
		
		public Booking borrowBook(Booking booking) throws Exception;
		
		public Booking returnBook(int bookingId, String comments) throws Exception;
		
		public boolean deleteBooking(int bookingId) throws Exception;
	}	
	
###### 借书记录 - BookingRecordInterface	

	public interface BookingRecordInterface {
	
		public BookingRecord addBookingRecord(BookingRecord bookingRecord) throws Exception;
		
		public List<BookingRecord> findBookingRecords(int bookingId) throws Exception;
	}
	
###### 消息 - Message

	public interface MessageInterface {
		
		public Message findReceiveMessages(int userId) throws Exception;
		
		public Message findSendMessages(int userId) throws Exception; 
		
		public Message addMessage(int fromUserId, int toUserId, String title, String content) throws Exception;
		
		public Message addBatchMessages(int fromUserId, int[] toUserIds, String title, String content) throws Exception;
		
		public boolean deleteMessages(int[] messageIds) throws Exception;
	}
	
###### 用户 - User

	public interface UserInterface {
		
		public List<User> findUsers() throws Exception;
		
		public User addUser(String corpId, String name) throws Exception;
		
		public boolean deleteUser(int userId) throws Exception;
		
		public boolean deleteUser(String corpId) throws Exception;
	}	
	
###### 角色 - Role

	public interface RoleInterface {
		
		public List<Role> findRoles() throws Exception;		
	}
	
###### 资源 - Resource

	public interface ResourceInterface {
		
		public List<Resource> findResources() throws Exception;	
	}
	
###### 权限 - Permission

	public interface PermissionInterface {
		
		public List<Permission> findPermissions() throws Exception;		
	}
	
###### 角色资源权限	- RoleResourcePermissionInterface

	public interface RoleResourcePermissionInterface {
		
		public List<ResourcePermission> findRoleResourcePermissions(int roleId) throws Exception;		
	}
	
###### 用户角色 - UserRoleInterface

	public interface UserRoleInterface {
		
		public List<Role> findUserRoles(int userId) throws Exception;		
	}	
