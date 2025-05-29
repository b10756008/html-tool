import os
import glob
import pandas as pd,openpyxl

import requests
from zeep import Client
from zeep.transports import Transport
import time
import subprocess
from datetime import datetime

from sqlalchemy import create_engine,inspect

from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options
from chrome_version import check_chrome_version

import pyautogui

server = '10.34.67.180' #IP:port 
DBuser = 'mcfab2'
password = '*mcfab2*'
engine = create_engine(f'mysql+pymysql://{DBuser}:{password}@{server}/manage_system')

current_date = datetime.now()

def get_data():
    folder_path = 'C:\\Users\\JingwenWang\\Downloads'  # 替換成你的資料夾路徑
    # 找到所有 Excel 檔案 (.xls 和 .xlsx)
    excel_files = glob.glob(os.path.join(folder_path, '*.xlsx'))
    filtered_files = [file for file in excel_files if 'Overview_withoutPILC' in os.path.basename(file)]

    if filtered_files:
        # 取最新的excel資料
        latest_file = max(excel_files, key=os.path.getmtime)
        df = pd.read_excel(latest_file)

        # 修改columns
        new_columns = ['fab', 'actual', 'BP_expense', 'BP_current_month', 'BP_cumulative','ReTargetBP_expense','ReTargetBP_current_month','ReTargetBP_cumulative']
        df.columns = new_columns
        # 刪除原excel index
        df = df.drop(index=0).reset_index(drop=True)
        filter_list = ['C4A','C5E','L3C','L3D','L4A','L5AB','L5C','L5D','L6A','M01']
        filtered_df = df[df['fab'].isin(filter_list)]



def selenium():
    # 設置下載屬性
    chrome_options = Options()
    # chrome_options.add_argument("--headless")  # 啟用無頭模式
    # chrome_options.add_experimental_option("prefs", {
    #     "download.prompt_for_download": False,  # 下載時不提示
    #     "download.directory_upgrade": True,  # 如果目錄存在，則升級使用
    #     "safebrowsing.enabled": True,  # 啟用安全瀏覽
    #     "profile.default_content_setting_values.automatic_downloads": 1,
        
    # })

    driver = webdriver.Chrome(service=Service(chromedriver_path),options=chrome_options)  # 或者使用其他瀏覽器的驅動
    try:
        # 登入
        # 初始化 Selenium WebDriver
        login_url = 'http://autcpmoweb01.corpnet.auo.com/IE_COST_Front/Report/Overview'  # 請替換為正確的登錄 URL
        driver.get(login_url)
        # 點擊到登入頁面
        WebDriverWait(driver, 10).until(EC.element_to_be_clickable((By.CLASS_NAME, "swal2-confirm"))).click()

        # 輸入用戶名稱
        username_input = driver.find_element(By.ID, "login_username")
        username_input.send_keys("JingwenWang")  # 帳號
        # 輸入密碼
        password_input = driver.find_element(By.ID, "login_password")
        password_input.send_keys("Katie#19991002")  # 密碼

        # 點擊登入按鈕
        WebDriverWait(driver, 10).until(EC.element_to_be_clickable((By.ID, "btn_login"))).click()
        time.sleep(5)
        # 回到Overview頁面
        driver.get(login_url)

        # 點擊without PI/LC按鈕
        WebDriverWait(driver, 10).until(EC.element_to_be_clickable((By.ID, "overview_table_without"))).click()

        # 打開LCD1的隱藏列
        WebDriverWait(driver, 10).until(EC.element_to_be_clickable((By.XPATH, "(//i[contains(@class, 'fa-plus-square')])[2]"))).click()

        # 點擊下載
        WebDriverWait(driver, 10).until(EC.element_to_be_clickable((By.ID, "overview_download_xlsx"))).click()
        WebDriverWait(driver, 10).until(EC.element_to_be_clickable((By.ID, "overview_file_xlsx"))).click()
        # pyautogui.moveTo(850, 70, duration = 1)
        # pyautogui.click(clicks=2,interval=0.5,button='right')
        pyautogui.moveTo(785, 110, duration = 0.5)
        pyautogui.click(clicks=1, button='left')
        time.sleep(10)
    finally:
        # 關閉瀏覽器
        driver.quit()

try:
    chromedriver_path = check_chrome_version()
    if chromedriver_path != '':
        print(f"""當前版本可使用""")
        # selenium()
        get_data()
except Exception as e:
    print(e)
