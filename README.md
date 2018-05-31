# Slack-bot
A bot I designed to answer questions in slack! 
import praw
import json
import re
import sys

def creds():
    with open('creds.json') as data_file:    
        data = json.load(data_file)
        reddit = praw.Reddit(client_id=str(data[0]['client_id']),
                                   client_secret=str(data[0]['client_secret']),
                                   password=str(data[0]['password']),
                                   user_agent=str(data[0]['user_agent']),
                                   username=str(data[0]['username']))
        return reddit

def scrape_subreddit(subreddit_name):
    reddit = creds()
    subreddit = reddit.subreddit(subreddit_name)
    results = []

    for submission in subreddit.hot(limit=5):
        print (str(submission.title))

def key_words(subs):
    reddit = creds()
    subreddit = reddit.subreddit(subs)
    results = []
    match_list = ['DI','Damage Inc','Damage Incorporated']
    non_bmp_map = dict.fromkeys(range(0x10000, sys.maxunicode + 1), 0xfffd)
    for submission in subreddit.hot(limit=500):
        for term in match_list:
            if re.search(term, submission.title):
                try:
                    print(str(submission.title))
                except UnicodeEncodeError:
                    print('Invalid Unicode Detected, Skipping Post')
           
key_words('ALL')
