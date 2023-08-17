# Ads relevant to content with Adrelevantis bidder adapter
[Adrelevantis bidder adapater](https://docs.prebid.org/dev-docs/bidders/adrelevantis.html) sends keywords and IAB categories, returns native ads relevant to the keywords and categories, and displays via Google Ad Manager tag. 

Here is an example by providing keywords and IAB categories. You can change the keywords and IAB categories to see how relevant ads are displayed.

# Code

```
<html>
<head>
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
            keywords: keywrds,
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
          cpm: 1
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
          cpm: 1
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
          cpm: 1
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
          cpm: 1
        }
      }]
    }];
    
    googletag.cmd.push(function() {
      var slot1 = googletag.defineSlot('/21901351985/native_nature', 'fluid', 'div-1').addService(googletag.pubads());
      var slot1 = googletag.defineSlot('/21901351985/native_square', 'fluid', 'div-2').addService(googletag.pubads());
      var slot1 = googletag.defineSlot('/21901351985/native_horizontal', 'fluid', 'div-3').addService(googletag.pubads());
      var slot1 = googletag.defineSlot('/21901351985/native_vertical', 'fluid', 'div-4').addService(googletag.pubads());
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

bidFunc("Pets Dental Health,Pet,Pet dental", "/pets|/pets/cats");
</script>
</head>
  <body>
    <h2>Native Content-Driven Advertising Examples</h2>
    <div id='div-1'>
    </div><br>
    <div id='div-2' style="width: 30%">
    </div><br>
    <div id='div-3' style="width: 50%">
    </div><br>
    <div id='div-4' style="width: 25%">
    </div><br>
  </body>
</html>
```

If your page has content, you can use an AI service that identifies keywords and IAB Categories of the page automatically in real time, and then, provides them to the bid process. To do that, use the code below to replace the call bidFunc("Pets Dental Health,Pet,Pet dental", "/pets|/pets/cats") above.

```
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
          var keywrds = res["Keyword"].replaceAll(/\|/g,",");
          bidFunc(keywrds, cats)
        }
      }
    }
  };
  h.open("POST", "https://api.adrelevantis.com/getcatskeywords", true);
  h.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
  h.send(pload)
});
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
