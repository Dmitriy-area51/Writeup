# Captcha1 | the Missing Lake

we used tesseract.exe to get better answer:

```python
import pytesseract
from PIL import Image
from selenium import webdriver
import base64
import time

driver = webdriver.Firefox()

driver.get('https://captcha1.uctf.ir/')

for i in range(1, 10000):
    captcha_image = driver.find_element_by_css_selector('img[alt="Generated Image"]')
    captcha_image_data = captcha_image.get_attribute('src')
    captcha_image_base64 = captcha_image_data.split(',')[1]

    with open('captcha.png', 'wb') as file:
        file.write(base64.b64decode(captcha_image_base64))

    captcha_image_path = 'captcha.png'
    captcha_image = Image.open(captcha_image_path)

    pytesseract.pytesseract.tesseract_cmd = r'C:\Users\Dmitriy\Desktop\ocr\Tesseract-OCR\tesseract.exe'
    captcha_text = pytesseract.image_to_string(captcha_image, config='--psm 8 -c tessedit_char_whitelist=abcdefghijklmnopqrstuvwxyz0123456789')
    print("Captcha is: ", captcha_text)
    captcha_input = driver.find_element_by_name('captcha')
    captcha_input.clear()
    captcha_text = captcha_text[:-1]  
    captcha_input.send_keys(captcha_text)

    submit_button = driver.find_element_by_xpath('//button[@type="submit"]')
    submit_button.click()

time.sleep(120)
driver.quit()

```
