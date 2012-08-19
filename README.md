# Alfresco CORS Filter
This is a modified version of the CORS filter found at:
 http://software.dzhuvinov.com/cors-filter.htm
 
The main modification is the addition of `cors.allowOriginSuffixMatching` that, when set to `true` allows for partial matching of Origin suffices while still fully matching the URI scheme.
 
For example, assuming the following configuration:

		<init-param>
			<param-name>cors.allowOrigin</param-name>
			<param-value>https://example.com https://bar.com</param-value>
		</init-param>
		
		<init-param>
			<param-name>cors.allowOriginSuffixMatching</param-name>
			<param-value>true</param-value>
		</init-param>

These request origins *will match*:

	Origin: https://www.example.com
	Origin: https://example.com
	Origin: http://foo.bar.com

While the following request origins *will not match*:

	Origin: http://www.example.com
	Origin: http://example.com:8080
	Origin: https://foo.bar.com

###Sample web.xml for Salesforce.com:

	<filter>
		<!-- The CORS filter with parameters -->
		<filter-name>CORS</filter-name>
		<filter-class>com.thetransactioncompany.cors.CORSFilter</filter-class>
		
		<!-- Note: All parameters are options, if ommitted CORS Filter
		     will fall back to the respective default values.
		  -->
		<init-param>
			<param-name>cors.allowGenericHttpRequests</param-name>
			<param-value>true</param-value>
		</init-param>
		
		<init-param>
			<param-name>cors.allowOrigin</param-name>
			<param-value>https://salesforce.com https://force.com</param-value>
		</init-param>
		
		<init-param>
			<param-name>cors.allowOriginSuffixMatching</param-name>
			<param-value>true</param-value>
		</init-param>
		
		<init-param>
			<param-name>cors.supportedMethods</param-name>
			<param-value>GET, HEAD, POST, PUT, DELETE, OPTIONS</param-value>
		</init-param>
		
		<init-param>
			<param-name>cors.supportedHeaders</param-name>
			<param-value>Content-Type, X-Requested-With</param-value>
		</init-param>
		
		<init-param>
			<param-name>cors.exposedHeaders</param-name>
			<param-value>X-Test-1, X-Test-2</param-value>
		</init-param>
		
		<init-param>
			<param-name>cors.supportsCredentials</param-name>
			<param-value>true</param-value>
		</init-param>
		
		<init-param>
			<param-name>cors.maxAge</param-name>
			<param-value>3600</param-value>
		</init-param>

	</filter>

	<filter-mapping>
		<!-- CORS Filter mapping -->
		<filter-name>CORS</filter-name>
		<url-pattern>/cors-resource.html</url-pattern>
	</filter-mapping>