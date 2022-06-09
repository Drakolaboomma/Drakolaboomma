from selenium import webdriver
import time
import datetime
import telegram_send

driver = webdriver.Chrome()
driver.get("https://coinmarketcap.com/")


def BTC():
    btc_name = driver.find_element_by_xpath('//*[@id="__next"]/div/div[1]/div[2]/div/div/div[5]/table/tbody/tr[1]/td[3]/div/a').text
    btc_price = driver.find_element_by_xpath('//*[@id="__next"]/div/div[1]/div[2]/div/div/div[5]/table/tbody/tr[1]/td[4]/div/a').text
    btc_24h = driver.find_element_by_xpath('//*[@id="__next"]/div/div[1]/div[2]/div/div/div[5]/table/tbody/tr[1]/td[5]/span').text
    btc_7d = driver.find_element_by_xpath('//*[@id="__next"]/div/div[1]/div[2]/div/div/div[5]/table/tbody/tr[1]/td[6]/span').text
    time.sleep(10)
    current_time = str(datetime.datetime.now())
    telegram_send.send(messages=[current_time])
    telegram_send.send(messages=[btc_name, btc_price, btc_24h, "24h", btc_7d, "7d"])
    print(btc_name, btc_price, btc_24h, "24h", btc_7d, "7d")


while True:
    try:
        time.sleep(10)
        BTC()
    except Exception as err:
        telegram_send(messages=["webdriver Closed"])
        driver.quit()
        break
