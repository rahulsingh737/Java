RESTful web services in Java in Eclipse

create folders inside src\main\java\org\Locks\aws\APP directory
1. resources
2. services

web service structure, path and parameters,response type is to be defined inside a java class in resources like

@Path("/register")
public class dbResource {

	RegisterService registerService = new RegisterService();
	@Path("/{fname}/{lname}/{email}/{password}/{phone}/{age}/{location}")
	@POST
	@Produces(MediaType.TEXT_PLAIN)
	public String getResult(@PathParam("fname") String fname,
							@PathParam("lname") String lname,
							@PathParam("email") String email,
							@PathParam("password") String password,
							@PathParam("phone") String phone,
							@PathParam("age") String age,
							@PathParam("location") String location)
	{
		return registerService.getStatus(fname,lname,email,password,phone,age,location);
	}
	
	@GET
	@Produces(MediaType.TEXT_PLAIN)
	public String getMessage()
	{
		return "Get method";
	}
}



and main function and processing task should be defined inside a java class in servies like

public class RegisterService {
	public static String connection()
	{
		try {
			Class.forName("com.mysql.jdbc.Driver");
			System.out.println("Worked");
		} catch (ClassNotFoundException e) {
			// TODO Auto-generated catch block
			System.out.println("Error in connection:"+e.getMessage());
			return "error";
		}
		return "ok";
	}

	public static String connectionToMySql(String a,String b,String c,String d,String e,String f,String g)
	{
		String s=connection();
		
		if(s.equals("error"))
		{
			return s;
		}
		
		String host="jdbc:mysql://abc.xyz";
		String username="username";
		String password="password";
		
		try {
			Connection conn=DriverManager.getConnection(host,username,password);
			System.out.println("Working dude");
			
			Statement stmt = conn.createStatement();
			
			String sql = "INSERT INTO users "+"VALUES ('"+a+"','"+b+"','"+c+"','"+d+"','"+e+"','"+f+"','"+g+"')";
			stmt.executeUpdate(sql);
			conn.close();
		} catch (SQLException err) {
			// TODO Auto-generated catch block
			System.out.println("Error code:"+err.getErrorCode());
			if(err.getErrorCode()==1062)
			{
				return "already registered";
			}
			System.out.println("Error in insert:"+err.getMessage());
			return "error";
		}
		return "ok";
	}
	
	public String getStatus(String a,String b,String c,String d,String e,String f,String g)
	{
		String status = connectionToMySql(a,b,c,d,e,f,g);
		return status;
	}
}


//if JSON data is coming it can be set and get in a way like
public class Data {
	
	private String firstname;
	private String lastname;
	
	public String getfirstname()
	{
		return this.firstname;
	}
	
	public void setfirstname(String name)
	{
		this.firstname = name;
	}
	
	public String getlastname()
	{
		return this.lastname;
	}
	
	public void setlastname(String name)
	{
		this.lastname = name;
	}
}
