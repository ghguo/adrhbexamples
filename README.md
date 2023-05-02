# Ads relevant to content with Adrelevantis bidder adapter
[Adrelevantis bidder adapater](https://docs.prebid.org/dev-docs/bidders/adrelevantis.html) is the first adapter in prebid.js that sends content's IAB categories and content's ranked keywords from publisher side. 

Here, a few examples with various content types (Sports, Home and Garden, Entertainment and Cooking, etc) are given to demonstrate how to show ads relevant to page content using Adrelevantis bidder adapter. The following code extracts IAB Categories and ranked keywords of a page, sends the categories and keywords as part of bid request and displays ads.

Drop the following code to your page to display ads relevant to the page content.

# Code

```
<script async src="//www.googletagservices.com/tag/js/gpt.js"></script>
<script async src="//www.adrelevantis.com/pub/prebid.js"></script>
<script src="//www.adrelevantis.com/pub/contentdriventag.js"></script>
<script>
var adUnits = [
	{
	  code: 'div-4',
	  sizes: [
		[250, 250]
	  ],
	  mediaTypes: {
		native: {
		  title: {
			required: true
		  },
		  image: {
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
		  allowSmallerSizes: true
		}
	  }]
	},
	{
	  code: 'div-3',
	  sizes: [
		[250, 250]
	  ],
	  mediaTypes: {
		native: {
		  title: {
			required: true
		  },
		  image: {
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
		  allowSmallerSizes: true
		}
	  }]
	},
	{
	  code: 'div-2',
	  sizes: [
		[250, 250]
	  ],
	  mediaTypes: {
		native: {
		  title: {
			required: true
		  },
		  image: {
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
		  allowSmallerSizes: true
		}
	  }]
	},
	{
		code: '/21901351985/header-bid-tag-0',
		mediaTypes: {
			banner: {
				sizes: [[300, 250], [300, 600]]
			}
		},
		bids: [{
			bidder: 'adrelevantis',
			params: {
				placementId: 13144370,
				cpm: 0.50
			}
		}]
	}
];
var googletag = googletag || {};
googletag.cmd = googletag.cmd || [];
googletag.cmd.push(function() {
	googletag.pubads().disableInitialLoad();
});
var pbjs = pbjs || {};
pbjs.que = pbjs.que || [];

var adDivIds = ['div-4','div-3','div-2','/21901351985/header-bid-tag-0'];
document.addEventListener("DOMContentLoaded", function(event){ adrtags("D435C107A8844E15BAA5D4A9B7D94FC5", adUnits, adDivIds); });
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

Cooking: essential-french-onion-soup.html

Entertainment: laura-marano-new-single-let-me-cry.html

Home and Garden: the-impracticality-of-hardwood-flooring.html

Pets: how-you-can-maintain-your-pets-dental-health.html

Sports: big-ten-reporters-pick-michigan-to-win-league-title-in-2019.html
