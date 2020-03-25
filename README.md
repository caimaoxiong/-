# -*-coding:utf8-*-
import requests
from bs4 import BeautifulSoup
import threading
from multiprocessing import Pool
import time
proxy_ips=[]
header={'User-Agent':'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/535.1 (KHTML, like Gecko) Chrome/14.0.835.163 Safari/535.1'}
proxy_ips_url=r'C:\Users\caimx\Desktop\python\ips.txt'
def get_ips(i):
    url=r'http://www.xiladaili.com/gaoni/'+str(i)+'/'
    print(url)
    resp=requests.get(url,headers=header)
    soup=BeautifulSoup(resp.text,'html')
    ips_tr=soup.find_all('tr')
    for ip_tr in ips_tr:
        if ip_tr.find_all('td'):
            ip=ip_tr.find_all('td')[0].text
            proxy_ips.append(ip)
            
    
if __name__ =='__main__':
    time1=time.time()
    pool = Pool(1)
    pool.map(get_ips,[x for x in range(1,11)])
    pool.close()
    pool.join()
    with open(proxy_ips_url,'a') as f:
        for ip in proxy_ips:
            f.write(ip+'\n')
    time2=time.time()
    print(time1-time2)
    

                

