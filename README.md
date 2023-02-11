# -
23.02.12 정상적으로 작동함

from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By
from selenium.common.exceptions import NoSuchElementException
import time
import random

options = webdriver.ChromeOptions()

options.add_argument('--no-sandbox')
options.add_argument('--disable-dev-shm-usage')
options.add_argument("--user-agent=Mozilla/5.0 (iPhone; CPU iPhone OS 6_0 like Mac OS X) AppleWebKit/536.26 (KHTML, like Gecko) Version/6.0 Mobile/10A5376e Safari/8536.25")
options.add_argument("--start-maximized")

driver = webdriver.Chrome('chromedriver.exe', options=options)
driver.get('https://instagram.com')
driver.implicitly_wait(15)
print('로그인 진행중...')

btn= driver.find_elements(By.TAG_NAME, 'button')[1]
btn.click()

inputbox = driver.find_elements(By.TAG_NAME, 'input')[0]
inputbox.click()
inputbox.send_keys('durex_0.02')

inputbox = driver.find_elements(By.TAG_NAME, 'input')[1]
inputbox.click()
inputbox.send_keys('alqp2938!!')

inputbox.send_keys(Keys.ENTER)
time.sleep(random.uniform(4.0, 6.0))

# 원하는 태그 입력 및 누르고 싶은 좋아요의 수 입력
insta_tag = "좋아요반사"
like_cnt = 100

driver.get('https://www.instagram.com/explore/tags/{}/'.format(insta_tag))
time.sleep(random.uniform(4.0, 8.0))

new_feed = driver.find_elements(By.XPATH, '//article//img //ancestor :: div[2]')[9]
new_feed.click()

numoflike = 0
for i in range(like_cnt): 
    time.sleep(random.uniform(4.0, 6.0))
    span = driver.find_element(By.XPATH, '//*[@aria-label="좋아요" or @aria-label="좋아요 취소"]//ancestor :: span[2]')  
    like_btn = span.find_element(By.TAG_NAME, 'button')
    btn_svg = like_btn.find_element(By.TAG_NAME, 'svg') 
    svg = btn_svg.get_attribute('aria-label') 
    
    if svg == '좋아요' : 
        like_btn.click() 
        numoflike += 1
        print('좋아요를 {}번째 눌렀습니다.'.format(numoflike))
        time.sleep(random.uniform(1.0, 2.0))
    else :
        print('이미 작업한 피드입니다.')               
        time.sleep(random.uniform(4.0, 6.0))      

    if i < like_cnt-1 : 
        next_feed_xpath = driver.find_element(By.XPATH, '//*[@aria-label="다음" and @height="16"]//ancestor :: div[2]')
        next_feed = next_feed_xpath.find_element(By.TAG_NAME, 
    'button') 
        next_feed.click() 
        time.sleep(random.uniform(1.1, 3.0))
        driver.implicitly_wait(5)

print("좋아요 작업이 끝났습니다. 프로그램을 종료합니다.")
driver.quit()
