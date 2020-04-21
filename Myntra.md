package days21;

import java.util.List;
import java.util.concurrent.TimeUnit;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.interactions.Actions;


public class Myntra {

	public static void main(String[] args) throws InterruptedException {
		System.setProperty("webdriver.chrome.driver", "./drivers/chromedriver.exe");
		//Disable chrome notifications
		ChromeOptions obj=new ChromeOptions();
		obj.addArguments("--disable-notifications");
		ChromeDriver driver=new ChromeDriver(obj);
		//1) Load URL
		driver.get("https://www.myntra.com/");
		driver.manage().window().maximize();
		driver.manage().timeouts().implicitlyWait(20,TimeUnit.SECONDS);
		
		//2) Mouse over on WOMEN
		Actions action=new Actions(driver);
		WebElement Women = driver.findElementByXPath("//a[text()='Women']");
		action.moveToElement(Women).perform();
		Thread.sleep(3000);
		//3) Click Jackets & Coats
		driver.findElementByXPath("//a[@data-reactid='231']").click();
		Thread.sleep(3000);
		//4) Find the total count of item (top) 
		String str= driver.findElementByXPath("//span[@class='title-count']").getText();
		String text = str.replaceAll("\\D","");
		int totalcount=Integer.parseInt(text);
		System.out.println(totalcount);
		
		String str1=driver.findElementByXPath("//span[@class='categories-num'][1]").getText();
		String text1=str1.replaceAll("\\D","");
		int jackets=Integer.parseInt(text1);
		System.out.println("Jackets Count: "+jackets);
		
		String str2=driver.findElementByXPath("(//span[@class='categories-num'])[2]").getText();
		String text2=str2.replaceAll("\\D","");
		int coats=Integer.parseInt(text2);
		System.out.println("Coat Count: "+coats);
		int sumofcategories=jackets+coats;
		
		//5) Validate the sum of categories count matches
		if(sumofcategories==totalcount)
		{
			System.out.println("Total count is correct: "+totalcount);
		}
		else
		{
			System.out.println("Count Mismatched");
		}
		
		//6) Check Coats
		WebElement coatcheckbox = driver.findElementByXPath("(//div[@class='common-checkboxIndicator'])[2]");
		coatcheckbox.click();
		
		// 7) Click + More option under BRAND	
		driver.findElementByXPath("//div[@class='brand-more']").click();
		
		//8) Type MANGO and click check box
		driver.findElementByXPath("//input[@placeholder='Search brand']").sendKeys("Mango");
		Thread.sleep(2000);
		driver.findElementByXPath("//label[@class=' common-customCheckbox']").click();
		
		//9) Close the pop-up x
		driver.findElementByXPath("//span[@class='myntraweb-sprite FilterDirectory-close sprites-remove']").click();
		
		//10) Confirm all the Coats are of brand MANGO
		int s=0;

		List<WebElement> brand = driver.findElementsByXPath("//h3[@class='product-brand']");
		Thread.sleep(2000);
		for(WebElement eachbrand:brand)
		{	
			String Mangobrand=eachbrand.getText();
		
			if(Mangobrand.equalsIgnoreCase("Mango"))
				
				s=s+1;	
		}
		System.out.println("Total number of Mango coats: "+s);

		if(s==brand.size())
		{
			System.out.println("All Products are Mango");
		}
		
		//11) Sort by Better Discount
		action.moveToElement(driver.findElementByXPath("//span[@class='myntraweb-sprite sort-downArrow sprites-downArrow']")).perform();
		driver.findElementByXPath("//label[text()='Better Discount']").click();
		
		Thread.sleep(2000);
		//12) Find the price of first displayed item

		WebElement discountedprice = driver.findElementByXPath("//span[@class='product-discountedPrice'][1]");
		String price1= discountedprice.getText();
		String price2=price1.replaceAll("\\D","");
		int Amount=Integer.parseInt(price2);
		System.out.println("Amount is : "+Amount);

		//13) Mouse over on size of the first item
		action.moveToElement(driver.findElementByXPath("//span[@class='product-discountedPrice'][1]")).perform();
		Thread.sleep(3000);
		
		//14) Click on WishList Now
		driver.findElementByXPath("//span[@class='product-actionsButton product-wishlist product-prelaunchBtn'][1]").click();
		
		//15) Close Browser
		driver.close();
	}
}
