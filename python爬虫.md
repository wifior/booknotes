```python

import requests
from pyquery import PyQuery
import xlwt
import xlrd
import os.path

file = xlwt.Workbook(encoding='utf-8')
table = file.add_sheet('data')



headers = {
    'User-Agent': 'Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:67.0) Gecko/20100101 Firefox/67.0'
}
def get_outer_list(maxNum):
    list = []
    for i in range(1,maxNum+1):
        url = 'https://hz.lianjia.com/ershoufang/pg'+str(i)
        print('正在爬取的链接为: %s' %url)
        response = requests.get(url,headers=headers)
        print('正在获取第 %d 页房源' %i)
        doc = PyQuery(response.text)
        num = 0
        for item in doc('.sellListContent li').items():
            num += 1
            list.append(item.attr('data-lj_action_housedel_id'))
        print('当前页面房源共 %d 套' %num)
    return list



def get_inner_info(list):
    num = 0
    for i in list:
        value = []
        try:
            response = requests.get('https://hz.lianjia.com/ershoufang/' +str(i)+'.html',headers = headers)
            doc = PyQuery(response.text)

            base_li_item = doc('.base .content ul li').remove('.label').items()
            base_li_list = []
            for item in base_li_item:
                base_li_list.append(item.text())

            transaction_li_item = doc('.transaction .content ul li').items()
            transaction_li_list = []

            for item in transaction_li_item:
                transaction_li_list.append(item.children().not_('.label').text())


            value.append(doc('.unitPriceValue').remove('i').text())
            value.append(doc('.price .total').text()+'万')
            value.append(doc('.areaName .info').text())
            value.append(doc('.communityName .info').text())
            value.append(base_li_list[0])
            value.append(base_li_list[1])
            value.append(base_li_list[2])
            value.append(base_li_list[3])
            value.append(base_li_list[4])
            value.append(base_li_list[5])
            value.append(base_li_list[6])
            value.append(base_li_list[7])
            value.append(base_li_list[8])
            value.append(base_li_list[9])
            value.append(base_li_list[10])
            value.append(base_li_list[11])
            value.append(transaction_li_list[0])
            value.append(transaction_li_list[1])
            value.append(transaction_li_list[2])
            value.append(transaction_li_list[3])
            value.append(transaction_li_list[4])
            value.append(transaction_li_list[5])
            value.append(transaction_li_list[6])

            write_excel_xlsx(num,value)
            num += 1
            print(i,':写入完成')
        except:
            print(i,':写入异常')
            continue
    file.save('hourse.xls')


def write_excel_xlsx(i,value):
    num = 0
    for j in value:
        table.write(i,num,j)
        num+=1

def main():
    lists = get_outer_list(2)
    print('共计获取房源:',len(list(set(lists))))
    get_inner_info(list(set(lists)))

if __name__ == '__main__':
    main()
```

