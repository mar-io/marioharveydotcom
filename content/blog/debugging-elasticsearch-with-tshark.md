+++
date = "2016-12-26T00:13:45-05:00"
title = "debugging elasticsearch with tshark"
draft = true

+++

sudo tshark -i any -f "src port 9200 or dst port 9200" -T fields -e http.request.method -e http.request.uri -e elasticsearch.address.port -e ip.dst