参考：[Spring Boot整合UEditor，解决找不到上传文件的问题](http://blog.csdn.net/yry0304/article/details/53462974)

我按照上面集成，改动小，但UEditorController中的rootPath获取不对，上文中项目是war包，而我的是jar包，修改如下：
- UEditorController

~~~java
public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException,IOException {
        request.setCharacterEncoding( "utf-8" );
        response.setHeader("Content-Type" , "text/html");
        PrintWriter out = response.getWriter();
        /*ServletContext application=this.getServletContext();

        String rootPath = application.getRealPath( "/" );
        */

        String action = request.getParameter("action");
        String result = new ActionEnter( request,  "").exec();
        out.write( result );
       /* if( action!=null &&
                (action.equals("listfile") || action.equals("listimage") ) ){
            rootPath = rootPath.replace("\\", "/");
            result = result.replaceAll(rootPath, "/");
        }*/
    }
~~~
- ConfigManager

~~~java
private String getConfigPath () {
		return "static"+File.separator+"ueditor" + File.separator + ConfigManager.configFileName;
}

private String readFile ( String path ) throws IOException {

		StringBuilder builder = new StringBuilder();

		try {

			InputStreamReader reader = new InputStreamReader( new ClassPathResource(path).getInputStream(), "UTF-8" );//jar包中读取静态文件要用ClassPathResource.getInputStream（）
			BufferedReader bfReader = new BufferedReader( reader );

			String tmpContent = null;

			while ( ( tmpContent = bfReader.readLine() ) != null ) {
				builder.append( tmpContent );
			}

			bfReader.close();

		} catch ( UnsupportedEncodingException e ) {
			// 忽略
		}

		return this.filter( builder.toString() );

	}
~~~
