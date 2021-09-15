<h1 align="center">
Google News Scraper
</h1>


## Table of Contents
- [Installation](#installation)
- [Usage](#usage)
- [Config](#config)
  - [Required](#required)
  - [Commands](#commands)
- [Output](#output)
- [Examples](#examples)
  - [Search for articles that mention NFT but not Bitcoin](#search-for-articles-that-mention-nft-but-not-bitcoin)
  - [Search for articles that contain Bitcoin OR ethereum within the past hour](#search-for-articles-that-contain-bitcoin-or-ethereum-within-the-past-hour)
- [FAQ](#faq)
   - [How many results should I expect from the query?](#how-many-results-should-i-expect-from-the-query)
   - [Do I need to worry about Google blocking my IP address?](#do-i-need-to-worry-about-google-blocking-my-ip-address)


Google News Scraper is a lightweight Python package that scrapes the latest data from Google News. With this news scraper, you can simply enter a keyword or phrase and receive a list of JSON objects as the result.

## Installation

To scrape Google News headlines, Python package --pygooglenews-- will be used. You can check out the official package documentation here. To install the package, enter the following command:

```pip install pygooglenews```

## Usage

For you to get started, import the package into your repository and initiate the GoogleNews package.

```
from pygooglenews import GoogleNews
gn = GoogleNews()
```

To get the top stories, you can add:

```top = gn.top_news()```

## Config 

### Required 

The GoogleNews class has two required variables: lang and country. There are several available options you can combine and try out for this. Check out the official Google News webpage to confirm the options available. In the bottom left corner of the page, scroll down to the Language & region section to find the list of supported combinations.

### Commands
<table>
 <tr>
    <th>Search by top news</th>
    <td>gn.top_news()</td>
    <td>This search scrapes the result of the top stories on Google News.</td>
  </tr>
  <tr>
    <th>Search by topic headlines</th>
    <td>gn.topic_headlines('business')</td>
    <td> This allows you to search based on topic headlines. Available headlines include world, business, nation, technology, entertainment, sports, science, and health.</td>
  </tr>
  <tr>
    <th>Query search</th>
    <td>gn.search('Panda -China',
when = '6m')</td>
    <td>With query search, you can query based on a particular keyword or filter out options without the keyword. You can also set a time limit on the query.  The example above searches for the keyword Panda and filters out options that include the word ‘China’ over the past 6 months.</td>
  </tr>
    <tr>
    <th>Search by geographical location</th>
    <td>gn.geo_headlines('San Fran')</td>
    <td>This option will provide search results based on their geographical location. In the example, you can access the Google News feed from San Francisco.</td>
  </tr>
</table>

# Output 

The Google Scraper output returns an array of JSON objects. An example of an ethereum keyword search result is shown below:

```
 {
            "title": "Cardano, the cryptocurrency that wants to dethrone Ethereum - OI Canadian",
            "title_detail": {
                "type": "text/plain",
                "language": null,
                "base": "",
                "value": "Cardano, the cryptocurrency that wants to dethrone Ethereum - OI Canadian"
            },
            "links": [
                {
                    "rel": "alternate",
                    "type": "text/html",
                    "href": "https://oicanadian.com/cardano-the-cryptocurrency-that-wants-to-dethrone-ethereum/"
                }
            ],
            "link": "https://oicanadian.com/cardano-the-cryptocurrency-that-wants-to-dethrone-ethereum/",
            "id": "CBMiUmh0dHBzOi8vb2ljYW5hZGlhbi5jb20vY2FyZGFuby10aGUtY3J5cHRvY3VycmVuY3ktdGhhdC13YW50cy10by1kZXRocm9uZS1ldGhlcmV1bS_SAQA",
            "guidislink": false,
            "published": "Mon, 13 Sep 2021 21:46:22 GMT",
            "published_parsed": [
                2021,
                9,
                13,
                21,
                46,
                22,
                0,
                256,
                0
            ],
            "summary": "<a href=\"https://oicanadian.com/cardano-the-cryptocurrency-that-wants-to-dethrone-ethereum/\" target=\"_blank\">Cardano, the cryptocurrency that wants to dethrone Ethereum</a>&nbsp;&nbsp;<font color=\"#6f6f6f\">OI Canadian</font>",
            "summary_detail": {
    "type": "text/html",
                "language": null,
                "base": "",
                "value": "<a href=\"https://oicanadian.com/cardano-the-cryptocurrency-that-wants-to-dethrone-ethereum/\" target=\"_blank\">Cardano, the cryptocurrency that wants to dethrone Ethereum</a>&nbsp;&nbsp;<font color=\"#6f6f6f\">OI Canadian</font>"
            },
            "source": {
                "href": "https://oicanadian.com",
                "title": "OI Canadian"
            },
            "sub_articles": []
        }
``` 

# Examples 

## Search for articles that mention NFT but not Bitcoin

In this example, you’ll first import and initialize the pygooglenews package as described earlier. This example will be using a query search so the method to use here is search. 

```
from pygooglenews import GoogleNews

gn = GoogleNews()

s = gn.search('NFT -bitcoin')

print(s['feed'].title)
```

## Search for articles that contain Bitcoin OR Ethereum within the past hour

This example filters topic headlines that contain bitcoin or ethereum within the last one hour. To do this, you can use the “OR” query term in the google news scraper. It also provides the option of sorting only the news article within the past hour using the “when” query term--where h=hour, m=month, y=year. The script is as follows:

```
from pygooglenews import GoogleNews

gn = GoogleNews()

s = gn.search('intitle=bitcoin OR intitle=ethereum, when = '1h')

print(s['feed'].title)
``` 

NOTE: The “intitle” query term searches only documents containing the specified phrase in the document title. In the search query, intitle:WORD, word here is your search keyword. Ensure to leave no space between the intitle query term and the word that follows it.
 
## Extension To Run Command Within Terminal & Not Within The Script

Most times, when you run a Google scraper, you don’t want to make every single search query change within the script, that’s not very efficient. Rather, you’d prefer to run the query commands in the terminal. The pygooglenews scraper package only queries Google News within the script and returns the search result in the terminal. 

Also, you’d probably want to save the search results in a JSON file within your project repo. To do that, we’ll have to improve on our script. First, let’s add the following lines of code to our script to save the result in JSON.

```
def parse_args():
    parser = argparse.ArgumentParser(description='Google News Scraper')
    parser.add_argument('-k', '--keyword', default='top_news', help='Enter Keyword')
    parser.add_argument('-n', '--name', default='output', help='json file name of where you would want result to be saved')
    args = vars(parser.parse_args())

    return args
``` 

Basically, this function parses arguments from the terminal. Then, it validates or transforms them if necessary before using them as query parameters on the search_news function.

As shown in the code block above, you can use the -k or --keyword to specify a keyword to search for on Google News. If you don’t pass in any keyword query, it returns the default search option of just the top stories. In addition to that, you can use -n or --name to save the results in the JSON file.  

In the search_news function, you can set up the command terminal to scrape the top news of the day and save it in a JSON file within your project.
 
```
def search_news(args):
    # default GoogleNews instance
    gn = GoogleNews(lang='en', country='US')

    keyword = args['keyword'].strip()
    name = args['name'].strip()

    if keyword=='top_news':
        print('scraping top news of the day')
        top_news = gn.top_news()
        return output_json(top_news,name)

    print('scraping search word or phrase')
    data = gn.search(keyword)
    return output_json(data, name)

if __name__ == '__main__':
    args = parse_args()
    output = search_news(args)
    print(output)
``` 

Similarly, you can set it up to filter by topic_headlines. To do that, your script file should look like this:

```
from pygooglenews import GoogleNews
from outputs import output_json
import argparse

topics = ["WORLD", "NATION", "BUSINESS", "TECHNOLOGY", "ENTERTAINMENT", "SCIENCE", "SPORTS", "HEALTH" ]

def parse_args():
    parser = argparse.ArgumentParser(description='Google News Scraper')
    parser.add_argument('-t', '--topic', default='TECHNOLOGY', help='Enter headline topic')
    parser.add_argument('-n', '--name', default='output', help='json file name of where you would want result to be saved')
    args = vars(parser.parse_args())

    if args['topic'].strip().upper() not in topics:
        raise ValueError(f" keyword must be any of {topics}")


    return args

def search_news(args):
    # default GoogleNews instance
    gn = GoogleNews(lang='en', country='US')

    topic = args['topic'].strip().upper()
    name = args['name'].strip()

    print('scraping search word or phrase')
    data = gn.topic_headlines(topic)
    return output_json(data, name)

if __name__ == '__main__':
    args = parse_args()
    output = search_news(args)
    print(output)
``` 

To run this script, enter the following command in your terminal. You may adjust the arguments as you please.

```python gnewscraper-headlines.py -n entertainment-response -t entertainment```

Take note that the file used in this particular script is saved as gnewscraper-headlines.py. Ensure that the command indicates the name of the google news headlines python scraper file.

# FAQ

## How many results should I expect from the query? 
 
In our search results, we got up to 100 search results when there was no limit on the time. Keywords and locations heavily influence search results.

## Do I need to worry about Google blocking my IP address?

Depending on the tool you’re using, scraping Google News may result in a ban on your IP address for a few hours. In that case, be sure to use proxies to ensure the IP address changes regularly. 

However, you have nothing to worry about with the pygooglenews package. It accesses Google News data via the RSS and according to our test, you can access the RSS feed as much as you want without worrying about a ban.










