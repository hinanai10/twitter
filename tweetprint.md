username = input("username : ")    #ツイートを取得したいアカウントのusernameを入力
import os
import openpyxl as px
if os.path.exists( 'tweetsample{}.xlsx'.format(username)) == False :       #入力したusernameのファイルが既にあるか確認し無ければ作成
    book = px.Workbook()
    book.save('tweetsample{}.xlsx'.format(username)) 

import tweepy
from requests_oauthlib import OAuth1Session

consumer_key = 'XXXXXXXXXXXXXXXXXXXXXXX'       #Xの箇所に任意のキーを入力
consumer_secret = 'XXXXXXXXXXXXXXXXXXXXXXX'
access_token = 'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX'
access_token_secret = 'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX'

auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)

api = tweepy.API(auth ,wait_on_rate_limit=True, wait_on_rate_limit_notify=True)

i = int(0)
wb = px.load_workbook('tweetsample{}.xlsx'.format(username))
ws = wb.worksheets[0]
a = ws.max_row   #ファイルの一番下の行の場所を確認
public_tweets = tweepy.Cursor(api.user_timeline, id="{}".format(username)).items()

for tweet in public_tweets:
    ws.cell(row = a+i, column = 1, value = tweet.text )
    i += 1
    print(a+i)    #ツイート数カウント
    print(tweet.text)　　　#ツイート取得確認
    
    
wb.save('tweetsample{}.xlsx'.format(username))
