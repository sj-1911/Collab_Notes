```shell
%%capture
!apt-get install poppler-utils
!pip install pdf2image

from pdf2image import convert_from_path
import requests
import matplotlib.pyplot as plt
import numpy as np
from skimage.io import imread
from skimage.transform import resize

def plot(x):
    fig, ax = plt.subplots()
    im = ax.imshow(x, cmap='gray')
    ax.axis('off')
    fig.set size_inches(5, 5)
    plt.show()

def get_google_slide(url):
    url_head = "https://docs.google.com/presentation/d/"
    url_body = url.split('/')[5]
    page_id = url.split('.')[-1]
    return url_head + url_body + "/export/pdf?id=" + url_body + "&pageid=" + page_id

def get_slides(url):
    url = get_google_slide(url)
    r = requests.get(url, allow_redirects=True)
    open('file.pdf', 'wb').write(r.content)
    images = convert_from_path('file.pdf', 500)
    return images

Data_deck = "https://docs.google.com/presentation/d/1cK-nCep6wIEZRbR55LtWKM4svrW78qlnbe3Q9sOI_f8/edit#slide=id.g206f8279a60_0_5"

image_list = get_slides(Data_deck)

n = len(image_list)
