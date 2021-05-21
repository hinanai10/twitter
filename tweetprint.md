<p>username = input("username : ")    #ツイートを取得したいアカウントのusernameを入力  <br>
import os      <br>
import openpyxl as px    <br>
if os.path.exists( 'tweetsample{}.xlsx'.format(username)) == False :       #入力したusernameのファイルが既にあるか確認し無ければ作成  <br>
    book = px.Workbook()   <br>
    book.save('tweetsample{}.xlsx'.format(username)) 

import tweepy  <br>
from requests_oauthlib import OAuth1Session   <br>

consumer_key = 'XXXXXXXXXXXXXXXXXXXXXXX'       #Xの箇所に任意のキーを入力 <br>
consumer_secret = 'XXXXXXXXXXXXXXXXXXXXXXX'       <br>
access_token = 'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX'   <br>
access_token_secret = 'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX'   <br>

auth = tweepy.OAuthHandler(consumer_key, consumer_secret) <br>
auth.set_access_token(access_token, access_token_secret)  <br>

api = tweepy.API(auth ,wait_on_rate_limit=True, wait_on_rate_limit_notify=True)  <br>

i = int(0)   <br>
wb = px.load_workbook('tweetsample{}.xlsx'.format(username))  <br>
ws = wb.worksheets[0]  <br>
a = ws.max_row   #ファイルの一番下の行の場所を確認   <br>
public_tweets = tweepy.Cursor(api.user_timeline, id="{}".format(username)).items()  <br>

for tweet in public_tweets:   
    ws.cell(row = a+i, column = 1, value = tweet.text )
    i += 1
    print(a+i)    #ツイート数カウント  <br>
    print(tweet.text)　　　#ツイート取得確認   <br>
    
    
wb.save('tweetsample{}.xlsx'.format(username))</p>
