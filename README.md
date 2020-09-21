# first-sentiment

punctuation_chars = ["'", '"', ",", ".", "!", ":", ";", '#', '@']

def strip_punctuation(x):
    s=''
    for i in x:
        if i in punctuation_chars:continue
        s=s+i
    return s

def get_pos(y):
    count=0
    words=y.split()
    for i in words:
        i=strip_punctuation(i).lower()
        if i in positive_words:
            count+=1
    return count

def get_neg(y):
    count=0
    words=y.split()
    for i in words:
        i=strip_punctuation(i).lower()
        if i in negative_words:
            count+=1
    return count

def net_score(p,n):
    if p>=n:
        return p
    elif n>p:
        return -n
    
# lists of words to use
positive_words = []
with open("positive_words.txt") as pos_f:
    for lin in pos_f:
        if lin[0] != ';' and lin[0] != '\n':
            positive_words.append(lin.strip())


negative_words = []
with open("negative_words.txt") as pos_f:
    for lin in pos_f:
        if lin[0] != ';' and lin[0] != '\n':
            negative_words.append(lin.strip())

with open("project_twitter_data.csv","r") as hand:
    tweet_text=[]
    n_retweet=[]
    n_reply=[]
    for line in hand:
        line=line.strip()
        words=line.split(",")
        tweet_text.append(words[0])
        n_retweet.append(words[1])
        n_reply.append(words[2])
    
with open("resulting_data.csv","w") as han:
    
    header=['Number of Retweets', 'Number of Replies', 'Positive Score', 'Negative Score', 'Net Score']
    han.write('{}, {}, {}, {}, {}'.format(header[0],header[1],header[2],header[3],header[4]))
    han.write('\n')
    for i in range(1,len(tweet_text)):
        han.write('{}, {}, {}, {}, {}'.format(n_retweet[i],n_reply[i],get_pos(tweet_text[i]),get_neg(tweet_text[i]),net_score(get_pos(tweet_text[i]),get_neg(tweet_text[i]))))
        han.write('\n')
with open("resulting_data.csv","r") as han:    

    for line in han:
           print(line)
    
