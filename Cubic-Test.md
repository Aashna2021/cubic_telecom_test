# Cubic Telecom Interview Assessment
## Scenario  
Cubic are providing an in-car wifi data service to owners of a particular car brand. Owners will manage their data usage (including purchasing data) via a new website. The website consists of the following pages:_

* Login page (username, password, Login and forgot password)
* Reset Password Page
* User Home Page
* Purchase Data Page
* FAQ Page

## Questions

**Question 1:** _List the Page Objects and flow/functionality object need to implement the framework._

**Solution:** 

	1) LoginPage
		username: A WebElement to locate & enter text in the username textfield 
		password: A WebElement to locate & enter text in the password textfield 
		loginButton: A WebElement to locate & perform functions of the login button.
		resetPassword: A WebElement to locate & perform functions of the reset password button.
	
	2) ResetPasswordPage
		phoneNumOrEmail: A WebElement to locate & enter text in the textfield to verify valid phone number or email of the user.
		otp: A WebElement to locate and enter the number(otp) to validate the phone number or email.
		submitButton: A WebElement to locate & perform functions of the submit button to check if the otp is correct or not.
		password: A WebElement to locate & enter text in the password textfield to reset password.
		confirmPassword: A WebElement to locate & enter text in the password textfield to confirm the password.
		saveButton: A WebElement to locate & perform functions of the save button to save the new password.
		exitButton: A WebElement to locate & perform functions of the exit button to exit from the current page.
	
	3) UserHomePage
		myAccountCurrentBalance: A WebElement that shows current payment balance in floating point value.
		viewPaymentHistory: A locator for link to payment history page 
		searchDataPlanField: A locator for textfield to search the wifi products on webpage.
		searchDataPlanButton: A locator for button to search the wifi products on webpage.
		purchaseDataPlan: A Web/navigation link locator to search for the available Wifi plans.
		faqLink: A Web/navigation link locator to navigate to the frequently Asked Questions page.
		helpAndSupport: A Web/navigation link locator for the support from customer care and helpline numbers.
	
			
	4) PurchaseDataPage
		purchasePlanType: A locator for Radio buttons having options -prepaid/postpaid
		plansList: A locator for list showing all the plans
		viewPlan: A locator for button showing detail for a plan
		buyPlan: A locator for button to navigate to proceed to payments page

			
	5) FaqPage
		questionsLink: A locator for link to the frequently asked questions.
		questionCategory: A locator for the dropdown consisting options for different question categories.
		searchQuestionTextField: A locator for textfield to search the questions on webpage.
		searchQuestionButton: A locator for button to search the questions on webpage.

		


**Question 2:** *In C#, Java or Python create a page object for the login page (which includes the following page elements username, password, Login button and forgot password).*  
**Solution:**

        import org.openqa.selenium.WebDriver;  
        import org.openqa.selenium.support.PageFactory;
        import org.openqa.selenium.WebElement;  
        import org.openqa.selenium.support.FindBy;
          
        public class BasePage {  
            protected WebDriver driver;  
          
            public BasePage(WebDriver driver){  
                this.driver=driver;  
                PageFactory.initElements(driver,this);  
            }  
        }
        public class LoginPage extends BasePage {  
          
            @FindBy(id="username")  
            private WebElement username;  
          
            @FindBy(id="password")  
            private WebElement password;  
          
            @FindBy(id = "submit")  
            private WebElement loginButton;  
          
            @FindBy(css = ".btn-info")  
            private WebElement forgotPassword;  
          
            public LoginPage(WebDriver driver)  
            {  
                super(driver);  
            }  
            public void enterUsername(String usernameText){  
                this.username.sendKeys(usernameText);  
            }  
            public void enterPassword(String passwordText){  
                this.password.sendKeys(passwordText);  
            }  
          
            public UserHomePage performLogin(){  
                this.loginButton.click();  
                return new UserHomePage(driver);  
            }  
          
            public void pressForgotPassword(){  
                this.forgotPassword.click();  
            }  
        }
        
        public class UserHomePage extends BasePage{  
      
        @FindBy(xpath = "//h2[contains(text(), 'Welcome')]")  
        private WebElement welcomeUserHeader;  
      
        public UserHomePage(WebDriver driver) {  
            super(driver);  
        }  
      
        public String getUserHeaderText(){  
            return welcomeUserHeader.getText();  
        }  
      
    }
    

**Question 3:** *In the language used to create the login page object write a test script to automate a successful login test case.*

**Solution:**

    import org.openqa.selenium.WebDriver;  
    import org.openqa.selenium.chrome.ChromeDriver;  
    import org.testng.annotations.AfterClass;  
    import org.testng.annotations.BeforeClass; 
    import com.cubic.pages.LoginPage;  
    import com.cubic.pages.UserHomePage;  
    import org.testng.annotations.*;  
    import org.testng.Assert; 
      
      public class Utils {  
        public final static String BASE_URL="file:///Users/shaileshwadhwa/Documents/projects/AASHNA-PROJECTS/SELENUM/Web_Automation_Form/src/main/java/html/pages/app/login.html";  
        public final static String CHROME_DRIVER_LOCATION="src/test/resources/chromedriver";  
    }
    
    public class BaseTest {  
        protected WebDriver driver;  
      
        @BeforeClass  
      public void setup(){  
            System.setProperty("webdriver.chrome.driver", Utils.CHROME_DRIVER_LOCATION);  
            driver = new ChromeDriver();  
        }  
      
        @AfterClass  
      public void cleanup(){  
            driver.manage().deleteAllCookies();  
            driver.close();  
        }  
      
    }
    public class LoginTest extends BaseTest {  
      
        @Test(testName = "Test Successful Login")  
        public void loginform() {  
            String username = "Aashna01";  
            String password = "Pass@0123";  
      
            driver.get(Utils.BASE_URL);  
            LoginPage loginPage = new LoginPage(driver);  
      
            loginPage.enterUsername(username);  
            loginPage.enterPassword(password);  
            UserHomePage userHomePage = loginPage.performLogin();  
      
            String actualUserHeader = userHomePage.getUserHeaderText();  
            Assert.assertEquals(actualUserHeader, "Welcome " + username, "Test Failed!");  
        }  
      
    }
       
**Question 4:** *The test automation framework automatically updates the organisations test management system with the result of testing via an API. Identify a suitable framework to communicate with this API and explain how it might the be used.*

**Solution:**

The above framework uses TestNG to run the tests and generate reports.
The reports are generated in the XML format.
So, to communicate with the given API to update the Test Management tool, the information the XML reports will have to be parsed into the appropriate format that the API can consume. 
Assuming the API consumes in JSON format,
`json` library in Java can be used. Following is the Maven Dependency for it:

    <dependency>
    	    <groupId>org.json</groupId>
    	    <artifactId>json</artifactId>
    	    <version>20210307</version>
     </dependency>

This provides methods like `XML.toJSONObject(xml_file_string)` that can be used to convert XML into JSON as the API expects.

The API request can then be sent using frameworks like `Apache HttpClient` in Java.
The Apache HttpClient library in Java provides usable classes to perform various HTTP requests like GET/POST etc.
For. Eg. Following statements create a HttpClient and perform GET/POST requests:

    CloseableHttpClient  httpClient  = HttpClients.createDefault();

<u>GET Request Handling</u>

    HttpGet  request  =  new  HttpGet(url);

<u>POST Request Handling</u>

    HttpPost  request  =  new  HttpPost(url);
    CloseableHttpResponse  response  = httpClient.execute(request))




**Question 5:** *The website talks to an underlying API which handles communications with back end systems. Required backend functionality is not available when the website is deployed for testing. Describe how you might simulate the behaviour of the API and what tools you might use.*
**Solution:**

In this Scenario, to simulate the behaviour of API, one solution can be to use the  **POSTMAN tool** to simulate our API Service.
Postman provides a facility to save all the API requests (placed in a Postman Collection) into Examples that contain various HTTP requests & corresponding responses for the endpoint(s) -- 200, 404, 500 etc.
The Collection can then be Mocked to create a **Mock Server** that serves all those saved example responses through its own URL (and actual endpoint methods). 
**For E.g.** 
Consider our REST API Service has following endpoints:
| Request Type | URL |
|--|--|
| POST | http://ourservicebase/api/v2/resources |
| GET | http://ourservicebase/api/v2/resources |

POST request sets a resource and GET request fetches the resources.

Using the postman Mock Server, these endpoints will look like:
| Request Type | URL |
|--|--|
| POST | https://33c0741f-0a29-4f10-a283-8705044e705b.mock.pstmn.io/api/v2/resources |
| GET | https://33c0741f-0a29-4f10-a283-8705044e705b.mock.pstmn.io/api/v2/resources |

These endpoints respond the same way as original endpoints but the difference being the latter bring the Mocked response as set by us while creating the mock server.
The following link can provide more information on this:
https://blog.postman.com/simulate-a-back-end-with-postmans-mock-service/

Another Solution to our problem can be to Implement a Mocking Service using Java based frameworks like Mockito which provides a good API to mock the Methods in a Class that ultimately would mimic the functionality of the whole backend.
