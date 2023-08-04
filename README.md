# Ads relevant to content with Adrelevantis bidder adapter
[Adrelevantis bidder adapater](https://docs.prebid.org/dev-docs/bidders/adrelevantis.html) is the first adapter in prebid.js that sends content's IAB categories and content's ranked keywords from publisher side. 

Here, a few examples with various content types (Sports, Home and Garden, Entertainment and Cooking, etc) are given to demonstrate how to show ads relevant to page content using Adrelevantis bidder adapter. The following code extracts IAB Categories and ranked keywords of a page, sends the categories and keywords as part of bid request and displays ads.

Drop the following code to your page to display ads relevant to the page content.

# Code

```
<script async src="//www.googletagservices.com/tag/js/gpt.js"></script>
<script async src="//www.adrelevantis.com/pub/prebid.js"></script>
<script>
//Prebid.js and Google Ad Manager code
var googletag = googletag || {};
googletag.cmd = googletag.cmd || [];

var PREBID_TIMEOUT = 3000;
var FAILSAFE_TIMEOUT = 6000;

var pbjs = pbjs || {};
pbjs.que = pbjs.que || [];

function bidFunc(keywrds, cats)
{
  //Keywords and IAB Categories of the content are sent as bidder parameters
  pbjs.que.push(function() {
    pbjs.setBidderConfig({
      bidders: ['adrelevantis'],
      config: {
        ortb2: {
          site: {
            keywords: keywrds.join(','),
            ext: {
			  data: {category: cats}
			}
          }
        }
      }
    });

	var adUnits = [
	{
	  code: 'div-1',
	  mediaTypes: {
		native: {
		  sendTargetingKeys: false,
		  title: {
			required: true
		  },
		  image: {
			required: true
		  },
          clickUrl: {
            required: true
          },
		  sponsoredBy: {
			required: true
		  }
		}
	  },
	  bids: [{
		bidder: 'adrelevantis',
		params: {
		  placementId: 13232354,
		  allowSmallerSizes: true,
		  cpm: 0.9
		}
	  }]
	},
	{
	  code: 'div-2',
	  mediaTypes: {
		native: {
		  sendTargetingKeys: false,
		  title: {
			required: true
		  },
		  image: {
			required: true
		  },
          clickUrl: {
            required: true
          },
		  sponsoredBy: {
			required: true
		  }
		}
	  },
	  bids: [{
		bidder: 'adrelevantis',
		params: {
		  placementId: 13232354,
		  allowSmallerSizes: true,
		  cpm: 0.9
		}
	  }]
	},
	{
	  code: 'div-3',
	  mediaTypes: {
		native: {
		  sendTargetingKeys: false,
		  title: {
			required: true
		  },
		  image: {
			required: true
		  },
          clickUrl: {
            required: true
          },
		  sponsoredBy: {
			required: true
		  }
		}
	  },
	  bids: [{
		bidder: 'adrelevantis',
		params: {
		  placementId: 13232354,
		  allowSmallerSizes: true,
		  cpm: 0.9
		}
	  }]
	},
	{
	  code: 'div-4',
	  mediaTypes: {
		native: {
		  sendTargetingKeys: false,
		  title: {
			required: true
		  },
		  image: {
			required: true
		  },
          clickUrl: {
            required: true
          },
		  sponsoredBy: {
			required: true
		  }
		}
	  },
	  bids: [{
		bidder: 'adrelevantis',
		params: {
		  placementId: 13232354,
		  allowSmallerSizes: true,
		  cpm: 0.9
		}
	  }]
	},
	{
	  code: '/21901351985/header-bid-tag-0',
	  mediaTypes: {
		banner: {
		  sizes: [[728, 90]]
		}
	  },
	  bids: [{
		bidder: 'adrelevantis',
		params: {
		  placementId: 13144370,
		  cpm: 0.50
		}
	  }]
	}];
	
	googletag.cmd.push(function() {
	  var slot1 = googletag.defineSlot('/21901351985/native_horizontal', 'fluid', 'div-1').addService(googletag.pubads());
	  var slot1 = googletag.defineSlot('/21901351985/native_vertical', 'fluid', 'div-2').addService(googletag.pubads());
	  var slot1 = googletag.defineSlot('/21901351985/native_square', 'fluid', 'div-3').addService(googletag.pubads());
	  var slot1 = googletag.defineSlot('/21901351985/native_nature', 'fluid', 'div-4').addService(googletag.pubads());
	  var slot2 = googletag.defineSlot('/21901351985/header-bid-tag-0', [[728, 90]], '/21901351985/header-bid-tag-0').addService(googletag.pubads());
	  googletag.pubads().disableInitialLoad();
	  googletag.pubads().enableSingleRequest();
	  googletag.enableServices();
	});

    pbjs.addAdUnits(adUnits);
    pbjs.requestBids({
      bidsBackHandler: initAdserver,
      timeout: PREBID_TIMEOUT
    });
  });

  function initAdserver(bids) {
    if (pbjs.initAdserverSet) return;
    
	googletag.cmd.push(function() {
      pbjs.que.push(function() {
        pbjs.setTargetingForGPTAsync();
        googletag.pubads().refresh();
      });
    });
	
    pbjs.initAdserverSet = true;
  }
  
  setTimeout(function(bids) {
    initAdserver(bids);
  }, FAILSAFE_TIMEOUT);
};

//Content-Driven Advertising needs access to content. So, wait DOMContentLoaded event to start the bid process
document.addEventListener("DOMContentLoaded", function(event){   
  //Content-Driven Advertising refers to individual pages
  //Set referrer to no-referrer-when-downgrade to ensure safety while providing page path
  if (document.querySelector('meta[name="referrer"]') === null){
	var meta = document.createElement('meta');
	meta.name = "referrer";
	meta.content = "no-referrer-when-downgrade";
	document.getElementsByTagName('head')[0].appendChild(meta);
  }
  else {
	document.querySelector('meta[name="referrer"]').content = "no-referrer-when-downgrade";
  }
  
  var q = document.getElementsByTagName('body')[0].innerText;
  
  var pload = 'q=' + encodeURIComponent(q);
  var h = new XMLHttpRequest();
  h.onreadystatechange = function () {
    if (4 === this.readyState) {
      if (200 === this.status) {
        var res = JSON.parse(this.responseText);
        if (res != null){
		  var cats = res["Category"];
		  var keywrds = [];
		  res["Keyword"].split("|").forEach((itm) => {
			keywrds.push(itm);
		  });
  
		  bidFunc(keywrds, cats)
		}
      }
    }
  };
  h.open("POST", "https://api.adrelevantis.com/getcatskeywords", true);
  h.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
  h.send(pload)
});
</script>
```

Put the above code in the page ```<head>``` section. And make sure your page has the div tags mentioned in the code. For example, inside your page, there are div tags something like
```
<div id='div-4'>
</div>
```
...

```
<div id='div-3'>
</div>
```
...

```
<div id='div-2'>
</div>
```
...

```
<div id='/21901351985/header-bid-tag-0'>
	<script type='text/javascript'>
		googletag.cmd.push(function() {
			googletag.display('/21901351985/header-bid-tag-0');
		});
	</script>
</div>
```
# Examples
Here are a few content types (IAB Categories) in the /ContentExamples.

Pets: how-you-can-maintain-your-pets-dental-health.html.
[Click to see the live page](https://www.adrelevantis.com/hb/how-you-can-maintain-your-pets-dental-health.html)

Cooking: essential-french-onion-soup.html.
[Click to see the live page](https://www.adrelevantis.com/hb/essential-french-onion-soup.html)

Entertainment: laura-marano-new-single-let-me-cry.html.
[Click to see the live page](https://www.adrelevantis.com/hb/laura-marano-new-single-let-me-cry.html)

Home and Garden: the-impracticality-of-hardwood-flooring.html.
[Click to see the live page](https://www.adrelevantis.com/hb/the-impracticality-of-hardwood-flooring.html)

Sports: big-ten-reporters-pick-michigan-to-win-league-title-in-2019.html.
[Click to see the live page](https://www.adrelevantis.com/hb/big-ten-reporters-pick-michigan-to-win-league-title-in-2019.html)
