+++
date = "2016-12-26T00:13:45-05:00"
title = "Using tshark to debug elasticsearch response payloads"
draft = false

+++

Recently, I ran into an issue where requests to elasticsearch were overwhelming our cluster. This was causing a noticable drop in app performance. After much digging, I realized I wanted to see the payload of the elasticsearch response to better understand what the queries were returning. We discovered that elasticsearch queries were returning way more data than was needed to power the page. By discovering this we were able to drastically improve app performance and reduce stress on elasticsearch. Below is the command that helped us get to that conclusion.

Note the following only works in the event you are making http api requests to elasticsearch:

`sudo tshark -i any -f "src port 9200 or dst port 9200" -T fields -e http.request.method -e http.request.uri -e elasticsearch.address.port -e ip.dst`

Happy elasticsearch debugging!
