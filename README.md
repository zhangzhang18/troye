# -*- coding:utf-8 -*-
import re
import urllib
import os
import time


def urlopen(url):
    response = urllib.urlopen(url)
    html = response.read()
    # data = html.decode('UTF-8')
    return html


def getImg(html):
    reg = r'src="(.+?\.jpg)" pic_ext'
    imgre = re.compile(reg)
    print imgre
    imglist = imgre.findall(html)
    x = 0
    for imgurl in imglist:
        urllib.urlretrieve(imgurl, '%s.jpg' % x)
        x = x + 1
        print x


def schedule(a, b, c):
    per = 100.0 * a * b / c
    if per > 100:
        per = 100
    print '%.2f%%' % per


def down(html):
    reg = r'class="BDE_Image" src="(.+?\.jpg)"'
    imgre = re.compile(reg)
    imglist = re.findall(imgre, html)
    t = time.localtime(time.time())
    foldername = str(t.__getattribute__("tm_year")) + "-" + str(t.__getattribute__("tm_mon")) + "-" + str(
        t.__getattribute__("tm_mday"))
    picpath = 'D:\\ImageDownload\\%s' % (foldername)

    if not os.path.exists(picpath):  # 路径不存在时创建一个
        os.makedirs(picpath)
    os.chdir(picpath)

    x = 0
    for imgurl in imglist:
        target = picpath + '\\%s.jpg' % x
        print 'Downloading image to location: ' + target + '\nurl=' + imgurl
        image = urllib.urlretrieve(imgurl, target, schedule)
        x += 1
    return image


if __name__ == '__main__':
    url = urlopen("http://tieba.baidu.com/p/4742738558")
    down(url)
    print "Download has finished."
