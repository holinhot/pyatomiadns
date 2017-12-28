pyatomiadns python3
===========

Python Api to atomiadns

Docs to be found here: https://pyatomiadns.readthedocs.org/en/latest/

This is work in progress.

fork from https://github.com/sejo/pyatomiadns

add client.py support python3 replace urllib

for use example:
```
import json
from client3 import AtomiaClient




api_url = 'http://localhost/pretty/atomiadns.json/'

ac = AtomiaClient(api_url,"dns@holin.net", "pass")
NAMESERVER = ["ns1.holin.net.","ns2.holin.net."]
SOA_EMAIL = "dns.holin.net."

def add_zone(zone):

    if not zone:
        return "cannot add an empty zone"

    try:
        ac.AddZone(zone, 3600, NAMESERVER[0],
                       SOA_EMAIL, 10800, 3600, 604800, 86400,
                       ["%s." % ns for ns in NAMESERVER], "default")
        return "Done"
    except Exception as e:
        return e





def add_record(zone,name,ttl,rdata,rtype):

    if not zone or not name or not ttl or not rdata or not rtype:
        return "Not enough information"
    records = [
        {
            "class": "IN",
            "ttl": ttl,
            "label": name,
            "rdata": rdata,
            "type": rtype
        }
    ]
    try:
        res_json = ac.AddDnsRecords(zone, records)
        res = json.loads(res_json)
        if 'error_message' in res:
            return res['error_message']

        return "ok"
    except Exception as e:
        return e

def getdnsrecords(zone, label):
    if not zone or not id:
        return "Not enough information"
    try:
        res = json.loads(ac.GetDnsRecords(zone, label))
        if 'error_message' in res:
            return res['error_message']
        return res

    except Exception as e:
        return e

def remove_record(zone,label,id):

    if not zone or not id:
        return "Not enough information"


    records = json.loads(ac.GetDnsRecords(zone, label))
    for record in records:
        if record['id'] == id:
            # remove this one
            try:
                res = json.loads(ac.DeleteDnsRecords(zone, [record]))
                if 'error_message' in res:
                    return res['error_message']
                return "ok"

            except Exception as e:
                return e
        return "Record not found"


def remove_recordtest(zone,label,type):

    if not zone or not label:
        return "Not enough information"


    records = json.loads(ac.GetDnsRecords(zone, label))
    return_data = []
    for record in records:
        if record['type'] == type:
            # remove this one
            try:
                res = json.loads(ac.DeleteDnsRecords(zone, [record]))
                if 'error_message' in res:
                    return_data.append(res['error_message'])
                return_data.append(res)

            except Exception as e:
                return_data.append(e)
        else:
            return_data.append("Type does not match")
    return return_data







print(add_record("test.com","testapi","3600","1.1.1.2","A"))

print(remove_recordtest("test.com","@","A"))
#print(getdnsrecords("test.com","@"))

#print(add_zone("test2.com"))

```




