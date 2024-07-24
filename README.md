from selenium import webdriver
import time

# Initialize Chrome WebDriver
driver = webdriver.Chrome('/path/to/chromedriver')

# Navigate to the website
driver.get('https://www.com.gov.in/')

try:
    # Store the main window handle
    main_window = driver.current_window_handle
    
    # List to keep track of new window handles
    new_windows = []
    
    # Function to click and handle new windows
    def click_and_open_new_window(xpath):
        # Find the element and click
        element = driver.find_element_by_xpath(xpath)
        element.click()
        
        # Wait for new window to open
        time.sleep(2)  # Adjust the sleep time as necessary
        
        # Switch to new window
        for window_handle in driver.window_handles:
            if window_handle != main_window and window_handle not in new_windows:
                driver.switch_to.window(window_handle)
                new_windows.append(window_handle)
                break
    
    # Click on "Create", "FAQ", and "Partners" anchor tags
    click_and_open_new_window("//a[contains(text(), 'Create')]")
    click_and_open_new_window("//a[contains(text(), 'FAQ')]")
    click_and_open_new_window("//a[contains(text(), 'Partners')]")
    
    # Display window/frame IDs
    print("Window/Frame IDs:")
    for window_id in new_windows:
        print(window_id)
    
    # Close the new windows and switch back to main window
    for window_id in new_windows:
        driver.switch_to.window(window_id)
        driver.close()
    
    # Switch back to main window
    driver.switch_to.window(main_window)
    
    # Navigate back to the home page
    driver.get('https://www.com.gov.in/')
    
finally:
    # Close the main browser session
    driver.quit()
