# 🥕 MVC
- MVC 패턴은 애플리케이션을 개발할 때 사용하는 디자인 패턴
- 애플리케이션의 개발 영역을 MVC(Model, View, Controller)로 구분하여 각 역할에 맞게 코드를 작성하는 개발 방식
- MVC 패턴을 도입하면서 UI 영역과 도메인(비즈니스 로직) 영역으로 구분되어 서로에게 영향을 주지 않으면서 개발과 유지보수를 가능하게 한다.
  - 유지보수가 편해지는 코드 구성 방식

## Model
- Login(Business & DB)을 처리하기 위한 모든 것.
- Controller로 부터 넘어온 data를 이용하여 이를 수정하고 그에 대한 결과를 다시 Controller에 return 한다(DAO, Service)

## view
- 모든 화면 처리를 담당
- Client의 요청에 대한 결과뿐 아니라 Controller에 요청을 보내는 화면단도 jsp에서 처리한다.
- Login처리를 위한 java code는 사라지고 결과 출력을 위한 code만 존재(JSP)

## Controller
- Client의 요청을 분석하여 Logic처리를 위한 Model단을 호출한다.
- return받은 결과(data)를 필요에 따라 request, session등에 저장하고 redirect또는 forward 방식으로 jsp(view)page를 출력한다.(Servlet)

![image](https://github.com/user-attachments/assets/97a0e264-e4d1-4498-a37d-e6aab7974bb9)

![image](https://github.com/user-attachments/assets/715ffc84-31fb-46b7-ada8-5e3c9f0cacd8)


### BookVO.java
``` java
package vo;

import java.io.Serializable;

public class BookVO implements Serializable{
	
	private int bookno; // NUMBER(4) PRIMARY KEY,
	private String title; // VARCHAR2(40),
	private String publisher; // VARCHAR2(40),
	private int price; // NUMBER(8)
	
	public BookVO() {	}

	public BookVO(int bookno, String title, String publisher, int price) {
		this.bookno = bookno;
		this.title = title;
		this.publisher = publisher;
		this.price = price;
	}

	public int getBookno() {
		return bookno;
	}

	public void setBookno(int bookno) {
		this.bookno = bookno;
	}

	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
	}

	public String getPublisher() {
		return publisher;
	}

	public void setPublisher(String publisher) {
		this.publisher = publisher;
	}

	public int getPrice() {
		return price;
	}

	public void setPrice(int price) {
		this.price = price;
	}

	@Override
	public String toString() {
		return "BookVO [bookno=" + bookno + ", title=" + title + ", publisher=" + publisher + ", price=" + price + "]";
	}

	
}
```

### BooKDAO.java
``` java
package dao;

import java.util.List;

import vo.BookVO;

public interface BookDAO {
	 List<BookVO> bookList();
	 void bookAdd(BookVO vo);
	 void deleteBook(int bookno);
	 void updateBook(BookVO vo);
	 BookVO getBook(int no) ;

	 List<BookVO> searchBook(String condition,String keyword);
}
```

### BookDAOImple.java
``` java
package dao;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.ArrayList;
import java.util.List;

import util.DBUtil;
import vo.BookVO;

public class BookDAOImpl implements BookDAO{

	public List<BookVO> bookList() {
		List<BookVO> list = new ArrayList<BookVO>();
		String sql = "select * from book order by bookno desc";
		             //"select * from book order by bookno desc limit 1 , 3"

		Connection con = null;
		PreparedStatement ps = null;
		ResultSet rs = null;
		
		try {
			con = DBUtil.getConnection();
			ps = con.prepareStatement(sql);
			rs = ps.executeQuery();
			while(rs.next()) {
				BookVO b = new BookVO();
				b.setBookno(rs.getInt("bookno"));
				b.setPrice(rs.getInt("price"));
                b.setPublisher(rs.getString("publisher"));
                b.setTitle(rs.getString("title"));
                list.add(b);
			}
			
		} catch (Exception e) {
			System.out.println(e);
		}finally {
			DBUtil.close(rs,ps,con);
		}
		return list;
	}

	public void bookAdd(BookVO vo) {
        String sql = "insert into book (title,publisher,price) " + 
        		"values (?,?,?)";
		
		Connection con = null;
		PreparedStatement ps = null;
		ResultSet rs = null;
		
		try {
			con = DBUtil.getConnection();
			ps = con.prepareStatement(sql);
			
			ps.setString(1, vo.getTitle());
			ps.setString(2, vo.getPublisher());
			ps.setInt(3, vo.getPrice());
			
			int i = ps.executeUpdate();
			if(i == 0) throw new Exception("등록 실패");
		} catch (Exception e) {
			System.out.println(e);
		}finally {
			DBUtil.close(rs,ps,con);
		}
	}
	public BookVO getBook(int no) {
		String sql = "select * from book where bookno = ?";
		
		Connection con = null;
		PreparedStatement ps = null;
		ResultSet rs = null;
		BookVO b = null;
		
		try {
			con = DBUtil.getConnection();
			ps = con.prepareStatement(sql);
			ps.setInt(1, no);
			rs = ps.executeQuery();
			while(rs.next()) {
				b = new BookVO();
				b.setBookno(rs.getInt("bookno"));
				b.setPrice(rs.getInt("price"));
                b.setPublisher(rs.getString("publisher"));
                b.setTitle(rs.getString("title"));
			}
			
		} catch (Exception e) {
			System.out.println(e);
		}finally {
			DBUtil.close(rs,ps,con);
		}
		return b;
	}

	public void deleteBook(int bookno) {
        String sql = "delete from book where bookno = ?";
		
		Connection con = null;
		PreparedStatement ps = null;
		ResultSet rs = null;
		
		try {
			con = DBUtil.getConnection();
			ps = con.prepareStatement(sql);
			ps.setInt(1, bookno);
			
			int i = ps.executeUpdate();
			if(i == 0) throw new Exception("삭제 오류 발생 ");
		} catch (Exception e) {
			System.out.println(e);
		}finally {
			DBUtil.close(rs,ps,con);
		}		
	}

	@Override
	public void updateBook(BookVO vo) {
		// TODO Auto-generated method stub
		String sql  = "UPDATE Book SET price = ?  WHERE bookno = ?";
		
		Connection con = null;
		PreparedStatement ps = null;
		ResultSet rs = null;
		int result = 0;

		try {
			con = DBUtil.getConnection();
			ps = con.prepareStatement(sql);
			// ? 세팅
			ps.setInt(1, vo.getPrice());
			ps.setInt(2, vo.getBookno());
			
			//실행 및 결과값 핸들링
            result = ps.executeUpdate();
            
		} catch (Exception e) {
			System.out.println(e);
		}finally {
			DBUtil.close(rs,ps,con);
		}
		
	}

	public List<BookVO> searchBook(String condition, String keyword) {
		List<BookVO> list = new ArrayList<BookVO>();

        String sql = "select * from Book where "+condition+" like ?  order by bookno desc";
        //select * from Book where publisher like '%한%' order by bookno desc;
        //select * from Book where publisher like concat('%','한','%') order by bookno desc;

		Connection con = null;
		PreparedStatement ps = null;
		ResultSet rs = null;
		
		try {
			con = DBUtil.getConnection();
			ps = con.prepareStatement(sql);
			ps.setString(1,"%"+ keyword+"%");
			
			rs = ps.executeQuery();
            while (rs.next()) {
				BookVO vo = new BookVO();
				vo.setBookno(rs.getInt("bookno"));
				vo.setTitle(rs.getString("title"));
				vo.setPublisher(rs.getString("publisher"));
				vo.setPrice(rs.getInt("price"));
				list.add(vo);
			}
			
		} catch (Exception e) {
			System.out.println(e);
		}finally {
			DBUtil.close(rs,ps,con);
		}
		
		return list;
	}
}
```

### BookService.java
``` java
package service;

import java.util.List;

import vo.BookVO;

public interface BookService {
	
	 List<BookVO> bookList();
	 void bookAdd(BookVO vo);
	 void deleteBook(int bookno);
	 void updateBook(BookVO vo);
	 BookVO getBook(int no) ;

	 List<BookVO> searchBook(String condition,String keyword);
}
```

### BookServiceImpl.java
``` java
package service;

import java.util.List;

import dao.BookDAO;
import vo.BookVO;

public class BookServiceImpl implements BookService{
    private BookDAO dao = null;
	
	public BookServiceImpl() {	}
	public BookServiceImpl(BookDAO dao) {
		this.dao = dao;
	}
	public BookDAO getDao() {
		return dao;
	}
	public void setDao(BookDAO dao) {
		this.dao = dao;
	}
	
	@Override
	public List<BookVO> bookList() {
		return dao.bookList();
	}
	@Override
	public void bookAdd(BookVO vo) {
		dao.bookAdd(vo);
	}
	@Override
	public void deleteBook(int bookno) {
		// TODO Auto-generated method stub
		dao.deleteBook(bookno);
	}
	@Override
	public void updateBook(BookVO vo) {
		// TODO Auto-generated method stub
		dao.updateBook(vo);
	}
	@Override
	public List<BookVO> searchBook(String condition, String keyword) {
		// TODO Auto-generated method stub
		return dao.searchBook(condition, keyword);
	}
	@Override
	public BookVO getBook(int no) {
		// TODO Auto-generated method stub
		return dao.getBook(no);
	}

}
```

### ListBookServlet.java
``` java
package servlet;

import java.io.IOException;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import dao.BookDAO;
import dao.BookDAOImpl;
import service.BookService;
import service.BookServiceImpl;
import vo.BookVO;

@WebServlet({ "/ListBookServlet", "/bookList.do" })
public class ListBookServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void service(HttpServletRequest request, HttpServletResponse response)throws ServletException, IOException {
		
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=UTF-8");
		
		BookDAO dao = new BookDAOImpl();
		BookService service = new BookServiceImpl(dao);
		
		List<BookVO> list = service.bookList();
		
		request.setAttribute("booklist", list);	
		String page = "/booklist.jsp";
		
		getServletContext().getRequestDispatcher(page).forward(request, response); 
//		request.getRequestDispatcher(page).forward(request, response);	.
		

		
		
	
	
	}

}
```
