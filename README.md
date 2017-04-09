# DNSBLs

> Having fun with Domain Name System Black Lists


## Table of Contents

* [RFCs](#rfcs)
* [How a DNSBL works](#how-a-dnsbl-works)
* [DNS A record](#dns-a-record)
* [DNS TXT record](#dns-txt-record)
* [Active DNS Black Lists](#active-dns-black-lists)
* [Sources](#sources)


## RFCs

Name | URL 
------------------------------------ | ---------------------------------------------
Clarifications to the DNS Specification | https://tools.ietf.org/html/rfc2181
Threat Analysis of the DNS | https://tools.ietf.org/html/rfc3833
DNS Blacklists and Whitelists | https://tools.ietf.org/html/rfc5782
Overview of Best Email DNSBL Operational Practices | https://tools.ietf.org/html/rfc6471


## How a DNSBL works

A DNSBL (Domain Name System Black List) is a list of IP address basically related to spam domain name sources. They are commonly used as a spam filter and can be very useful too for IP reputation and investigation.

**DNS Black List can cover much more than just spam activities, they may include :**

* Major Spam Source DNSBLs
* User Submission DNSBLs
* Paid Subscription DNSBLs
* Bogon, Hijacked & Zombie Network DNSBLs
* Open Relay DNSBLs
* Open Proxy DNSBLs
* DNS Blackhole Lists
* Open Formmail DNSBLs
* Unconfirmed Mailing Lists
* Right-Hand Side Blacklist *(Include domain names rather than IP addresses)*
* IP-Addresses who attacks Server/Honeypots
* Spamtrap Driven or Automated DNSBLs
* And more...

Each list has her own criteria, standards and policies. Some of them more trustworthy and respectable than others.

To know if an IP is present in a DNSBL, there are 2 DNS resource record types, A and TXT record.


### DNS A record

If an IP is not black listed in a DNSBL, the query will return `NXDOMAIN`.
If the IP is black listed, the query will return an address as code. The value of the last octet will indicate the category or the reason why this IP is black listed.

The thing is that, each DNSBL has her own value for each category in addition to have their own policies...

Here is an example of the return codes between 2 majors DNSBLs : [Spamhaus](https://www.spamhaus.org/) and [Sorbs](http://www.sorbs.net/)


#### Spamhaus

Return codes for [Spamhaus](https://www.spamhaus.org/faq/section/DNSBL%20Usage) :

Adresse | Detail 
------------------------------------ | ---------------------------------------------
127.0.0.2,3,9 | Static UBE sources, verified spam services (hosting or support) and ROKSO spammers
127.0.0.4-7 | Illegal 3rd party exploits, including proxies, worms and trojan exploits
127.0.0.10,11 | IP ranges which should not be delivering unauthenticated SMTP email.


#### SORBS

Return codes for [Sorbs](http://www.us.sorbs.net/using.shtml) :

Adresse | Detail 
------------------------------------ | ---------------------------------------------
127.0.0.2 | http.dnsbl.sorbs.net
127.0.0.3 | socks.dnsbl.sorbs.net
127.0.0.4 | misc.dnsbl.sorbs.net
127.0.0.5 | smtp.dnsbl.sorbs.net
127.0.0.6 | new/recent/old.spam.dnsbl.sorbs.net && spam/scalations.dnsbl.sorbs.net
127.0.0.7 | web.dnsbl.sorbs.net
127.0.0.8 | block.dnsbl.sorbs.net
127.0.0.9 | zombie.dnsbl.sorbs.net
127.0.0.10 | dul.dnsbl.sorbs.net
127.0.0.11 | badconf.rhsbl.sorbs.net
127.0.0.12 | nomail.rhsbl.sorbs.net
127.0.0.14 | noserver.dnsbl.sorbs.net
127.0.0.15 | virus.dnsbl.sorbs.net


So, basically, the last octet of the returned address is important, but he will have a different sense between each DNSBLs. You can find additional information with the TXT record.


#### Example

For a DNS A query with [Spamhaus](https://www.spamhaus.org/) and the IP `217.197.83.197`, we need to reverse the order of octets :
```bash
$ host 197.83.197.217.zen.spamhaus.org
197.83.197.217.zen.spamhaus.org has address 127.0.0.2
```

You can also use nslookup or dig :
```bash
$ nslookup $IP.zen.spamhaus.org
$ dig +short A $IP.zen.spamhaus.org
```

If you want to use a specific DNS server :
```bash
$ dig +short @DNSserver @IP.zen.spamhaus.org
$ host @IP.zen.spamhaus.org @DNSserver
$ nslookup @IP.zen.spamhaus.org @DNSserver
```


### DNS TXT record

The TXT record provides information on the reason an IP is black listed.

#### Example
```bash
$ host -t TXT 197.83.197.217.zen.spamhaus.org
197.83.197.217.zen.spamhaus.org descriptive text "https://www.spamhaus.org/sbl/query/SBL313862"
```

You can also use nslookup or dig :
```
$ nslookup -type=TXT $IP.zen.spamhaus.org 
$ dig +short TXT $IP.zen.spamhaus.org
```


## Active DNS Black Lists

> Today April 9th 2017, I found 494 active DNSBLs. 

I have done my test from multiple DNS server, including my own [Unbound](https://www.unbound.net/).

**Inactive DNSBL :**
* Did not answer with 5s timeout	
* `No nameswervers` answer
* No answer
* TXT record indicated that the list is not valid anymore (Dead DNSBL will always return a false positive)

**Active DNSBL :**
* DNS A record return a code and DNS TXT record return information
* `NXDOMAIN` answer which means the domain is not listed in the DNSBL


I filtered the full list to keep only active DNSBL, then I did my best to remove all white lists and the duplicates one.
For example, `zen.spamhaus.org` is the combination of all Spamhaus DNSBLs `SBL, SBLCSS, XBL and PBL` or `multi.uribl.com` is the combination of all uribl.com DNSBLs `black.uribl.com grey.uribl.com red.uribl.com and white.uribl.com`

Here is all unique DNSBLs I found to be active in alphabetical order. You can find this list in a [raw format here](https://raw.githubusercontent.com/zbetcheckin/DNSBLs/master/active_dnsbls.txt)


Nb | Name | :ballot_box_with_check:
------------------------------------ | ------------------------------------ | ------------------------------------
1 | 0outspam.fusionzero.com | :white_check_mark:
2 | 0spam.fusionzero.com | :white_check_mark:
3 | 0spam-killlist.fusionzero.com | :white_check_mark:
4 | 0spamtrust.fusionzero.com | :white_check_mark:
5 | 0spamurl.fusionzero.com | :white_check_mark:
6 | 3y.spam.mrs.kithrup.com | :white_check_mark:
7 | 88.blocklist.zap | :white_check_mark:
8 | abuse-contacts.abusix.org | :white_check_mark:
9 | abuse.rfc-clueless.org | :white_check_mark:
10 | abuse.rfc-ignorant.org | :white_check_mark:
11 | access.atlbl.net | :white_check_mark:
12 | access.redhawk.org | :white_check_mark:
13 | accredit.habeas.com | :white_check_mark:
14 | admin.bl.kundenserver.de | :white_check_mark:
15 | all.ascc.dnsbl.bit.nl | :white_check_mark:
16 | all.dnsbl.bit.nl | :white_check_mark:
17 | all.rbl.jp | :white_check_mark:
18 | all.rbl.webiron.net | :white_check_mark:
19 | all.s5h.net | :white_check_mark:
20 | all.spamrats.com | :white_check_mark:
21 | all.spam-rbl.fr | :white_check_mark:
22 | all.v6.ascc.dnsbl.bit.nl | :white_check_mark:
23 | apnic-main.bogons.dnsiplists.completewhois.com | :white_check_mark:
24 | arin-legacy-classb.bogons.dnsiplists.completewhois.com | :white_check_mark:
25 | arin-legacy-classc.bogons.dnsiplists.completewhois.com | :white_check_mark:
26 | arin-main.bogons.dnsiplists.completewhois.com | :white_check_mark:
27 | asiaspam.spamblocked.com | :white_check_mark:
28 | aspews.dnsbl.sorbs.net | :white_check_mark:
29 | aspews.ext.sorbs.net | :white_check_mark:
30 | assholes.madscience.nl | :white_check_mark:
31 | auth.spamrats.com | :white_check_mark:
32 | autowork.drbl.ks.cz | :white_check_mark:
33 | babl.rbl.webiron.net | :white_check_mark:
34 | backscatter.spameatingmonkey.net | :white_check_mark:
35 | badconf.rhsbl.sorbs.net | :white_check_mark:
36 | badnets.spameatingmonkey.net | :white_check_mark:
37 | bandwidth-pigs.monkeys.com | :white_check_mark:
38 | ban.zebl.zoneedit.com | :white_check_mark:
39 | b.barracudacentral.org | :white_check_mark:
40 | bb.barracudacentral.org | :white_check_mark:
41 | bitonly.dnsbl.bit.nl | :white_check_mark:
42 | blackhole.compu.net | :white_check_mark:
43 | blackholes.brainerd.net | :white_check_mark:
44 | blackholes.easynet.nl | :white_check_mark:
45 | blackholes.five-ten-sg.com | :white_check_mark:
46 | blackholes.sandes.dk | :white_check_mark:
47 | black.junkemailfilter.com | :white_check_mark:
48 | blacklist.fpsn.net | :white_check_mark:
49 | blacklist.hostkarma.com | :white_check_mark:
50 | blacklist.informationwave.net | :white_check_mark:
51 | blacklist.mail.ops.asp.att.net | :white_check_mark:
52 | blacklist.mailrelay.att.net | :white_check_mark:
53 | blacklist.sci.kun.nl | :white_check_mark:
54 | blacklist.sci.ru.nl | :white_check_mark:
55 | blacklist.sequoia.ops.asp.att.net | :white_check_mark:
56 | bl.blocklist.de | :white_check_mark:
57 | bl.blueshore.net | :white_check_mark:
58 | bl.borderworlds.dk | :white_check_mark:
59 | bl.deadbeef.com | :white_check_mark:
60 | bl.drmx.org | :white_check_mark:
61 | bl.emailbasura.org | :white_check_mark:
62 | bl.fmb.la | :white_check_mark:
63 | bl.ipv6.spameatingmonkey.net | :white_check_mark:
64 | bl.konstant.no | :white_check_mark:
65 | bl.mailspike.net | :white_check_mark:
66 | bl.mailspike.org | :white_check_mark:
67 | bl.mav.com.br | :white_check_mark:
68 | bl.mipspace.com | :white_check_mark:
69 | bl.nszones.com | :white_check_mark:
70 | block.ascams.com | :white_check_mark:
71 | block.blars.org | :white_check_mark:
72 | block.dnsbl.sorbs.net | :white_check_mark:
73 | blocked.asgardnet.org | :white_check_mark:
74 | blocked.hilli.dk | :white_check_mark:
75 | blocklist2.squawk.com | :white_check_mark:
76 | blocklist.squawk.com | :white_check_mark:
77 | bl.reynolds.net.au | :white_check_mark:
78 | bl.scientificspam.net | :white_check_mark:
79 | bl.score.senderscore.com | :white_check_mark:
80 | bl.shlink.orgdul.ru | :white_check_mark:
81 | bl.spamcannibal.org | :white_check_mark:
82 | bl.spamcop.net | :white_check_mark:
83 | bl.spameatingmonkey.net | :white_check_mark:
84 | bl.spamstinks.com | :white_check_mark:
85 | bl.spamthwart.com | :white_check_mark:
86 | bl.student.pw.edu.pl | :white_check_mark:
87 | bl.suomispam.net | :white_check_mark:
88 | bl.tolkien.dk | :white_check_mark:
89 | bogon.lbl.lagengymnastik.dk | :white_check_mark:
90 | bogons.cymru.com | :white_check_mark:
91 | bogons.dnsiplists.completewhois.com | :white_check_mark:
92 | bogusmx.rfc-clueless.org | :white_check_mark:
93 | bogusmx.rfc-ignorant.org | :white_check_mark:
94 | bsb.empty.us | :white_check_mark:
95 | bsb.spamlookup.net | :white_check_mark:
96 | cabl.rbl.webiron.net | :white_check_mark:
97 | cart00ney.surriel.com | :white_check_mark:
98 | catchspam.com | :white_check_mark:
99 | cbl.abuseat.org | :white_check_mark:
100 | cbl.anti-spam.org.cn | :white_check_mark:
101 | cblless.anti-spam.org.cn | :white_check_mark:
102 | cblplus.anti-spam.org.cn | :white_check_mark:
103 | cdl.anti-spam.org.cn | :white_check_mark:
104 | china.rominet.net | :white_check_mark:
105 | cidr.bl.mcafee.com | :white_check_mark:
106 | client-domain.sjesl.monkeys.com | :white_check_mark:
107 | cml.anti-spam.org.cn | :white_check_mark:
108 | combined.abuse.ch | :white_check_mark:
109 | combined-HIB.dnsiplists.completewhois.com | :white_check_mark:
110 | combined.rbl.msrbl.net | :white_check_mark:
111 | communicado.fmb.la | :white_check_mark:
112 | contacts.abuse.net | :white_check_mark:
113 | country-rirdata.dnsiplists.completewhois.com | :white_check_mark:
114 | crawler.rbl.webiron.net | :white_check_mark:
115 | csi.cloudmark.com | :white_check_mark:
116 | czdynamic.drbl.ks.cz | :white_check_mark:
117 | dbl.suomispam.net | :white_check_mark:
118 | db.rurbl.ru | :white_check_mark:
119 | db.wpbl.info | :white_check_mark:
120 | dev.null.dk | :white_check_mark:
121 | devnull.drbl.be.net.ru | :white_check_mark:
122 | dialup.blacklist.jippg.org | :white_check_mark:
123 | dialup.drbl.sandy.ru | :white_check_mark:
124 | dialups.mail-abuse.org | :white_check_mark:
125 | dialups.visi.com | :white_check_mark:
126 | dnsbl-0.uceprotect.net | :white_check_mark:
127 | dnsbl-1.uceprotect.net | :white_check_mark:
128 | dnsbl-2.uceprotect.net | :white_check_mark:
129 | dnsbl-3.uceprotect.net | :white_check_mark:
130 | dnsbl6.anticaptcha.net | :white_check_mark:
131 | dnsbl.abuse.ch | :white_check_mark:
132 | dnsbl.anticaptcha.net | :white_check_mark:
133 | dnsbl.antispam.or.id | :white_check_mark:
134 | dnsbl.calivent.com.pe | :white_check_mark:
135 | dnsbl.cbn.net.id | :white_check_mark:
136 | dnsblchile.org | :white_check_mark:
137 | dnsbl.clue-by-4.org | :white_check_mark:
138 | dnsbl.cobion.com | :white_check_mark:
139 | dnsbl.cyberlogic.net | :white_check_mark:
140 | dnsbl.delink.net | :white_check_mark:
141 | dnsbl.dronebl.org | :white_check_mark:
142 | dnsbl.forefront.microsoft.com | :white_check_mark:
143 | dnsbl.httpbl.org | :white_check_mark:
144 | dnsbl.inps.de | :white_check_mark:
145 | dnsbl.ioerror.us | :white_check_mark:
146 | dnsbl.justspam.org | :white_check_mark:
147 | dnsbl.kempt.net | :white_check_mark:
148 | dnsbl.madavi.de | :white_check_mark:
149 | dnsbl.mags.net | :white_check_mark:
150 | dnsbl.mailshell.net | :white_check_mark:
151 | dnsbl.mcu.edu.tw | :white_check_mark:
152 | dnsbl.net.ua | :white_check_mark:
153 | dnsbl.pagedirect.net | :white_check_mark:
154 | dnsbl.rangers.eu.org | :white_check_mark:
155 | dnsbl.rizon.net | :white_check_mark:
156 | dnsbl.rv-soft.info | :white_check_mark:
157 | dnsbl.rymsho.ru | :white_check_mark:
158 | dnsbl.sorbs.net | :white_check_mark:
159 | dnsbl.spfbl.net | :white_check_mark:
160 | dnsbl.technoirc.org | :white_check_mark:
161 | dnsbl.tornevall.org | :white_check_mark:
162 | dnsbl.webequipped.com | :white_check_mark:
163 | dnsbl.wpbl.pc9.org | :white_check_mark:
164 | dnsbl.zapbl.net | :white_check_mark:
165 | dnsrbl.org | :white_check_mark:
166 | dnsrbl.swinog.ch | :white_check_mark:
167 | dnswl.inps.de | :white_check_mark:
168 | dnswl.leisi.net | :white_check_mark:
169 | dob.sibl.support-intelligence.net | :white_check_mark:
170 | drone.abuse.ch | :white_check_mark:
171 | dronebl.noderebellion.net | :white_check_mark:
172 | dsn.bl.rfc-ignorant.de | :white_check_mark:
173 | dsn.rfc-clueless.org | :white_check_mark:
174 | dsn.rfc-ignorant.org | :white_check_mark:
175 | dssl.imrss.org | :white_check_mark:
176 | duinv.aupads.org | :white_check_mark:
177 | dul.dnsbl.borderware.com | :white_check_mark:
178 | dul.dnsbl.sorbs.net | :white_check_mark:
179 | dul.dnsbl.sorbs.netdul.ru | :white_check_mark:
180 | dul.orca.bc.ca | :white_check_mark:
181 | dul.pacifier.net | :white_check_mark:
182 | dul.ru | :white_check_mark:
183 | dynablock.easynet.nl | :white_check_mark:
184 | dynamic.dnsbl.rangers.eu.org | :white_check_mark:
185 | dyna.spamrats.com | :white_check_mark:
186 | dyndns.rbl.jp | :white_check_mark:
187 | dynip.rothen.com | :white_check_mark:
188 | dyn.nszones.com | :white_check_mark:
189 | elitist.rfc-clueless.org | :white_check_mark:
190 | endn.bl.reynolds.net.au | :white_check_mark:
191 | escalations.dnsbl.sorbs.net | :white_check_mark:
192 | eswlrev.dnsbl.rediris.es | :white_check_mark:
193 | eurospam.spamblocked.com | :white_check_mark:
194 | ex.dnsbl.org | :white_check_mark:
195 | exitnodes.tor.dnsbl.sectoor.de | :white_check_mark:
196 | exitnodes.tor.dnsbl.sectoor.dehttp.dnsbl.sorbs.net | :white_check_mark:
197 | feb.spamlab.com | :white_check_mark:
198 | fnrbl.fast.net | :white_check_mark:
199 | formmail.relays.monkeys.com | :white_check_mark:
200 | fresh10.spameatingmonkey.net | :white_check_mark:
201 | fresh15.spameatingmonkey.net | :white_check_mark:
202 | fresh.dict.rbl.arix.com | :white_check_mark:
203 | fresh.sa_slip.rbl.arix.com | :white_check_mark:
204 | fresh.spameatingmonkey.net | :white_check_mark:
205 | fulldom.rfc-clueless.org | :white_check_mark:
206 | geobl.spameatingmonkey.net | :white_check_mark:
207 | gl.suomispam.net | :white_check_mark:
208 | hbl.atlbl.net | :white_check_mark:
209 | helo-domain.sjesl.monkeys.com | :white_check_mark:
210 | hijacked.dnsiplists.completewhois.com | :white_check_mark:
211 | hil.habeas.com | :white_check_mark:
212 | hong-kong.rominet.net | :white_check_mark:
213 | hostkarma.junkemailfilter.com | :white_check_mark:
214 | hostkarma.junkemailfilter.com[brl] | :white_check_mark:
215 | httpbl.abuse.ch | :white_check_mark:
216 | http.dnsbl.sorbs.net | :white_check_mark:
217 | hul.habeas.com | :white_check_mark:
218 | iadb2.isipp.com | :white_check_mark:
219 | iadb.isipp.com | :white_check_mark:
220 | iana-classa.bogons.dnsiplists.completewhois.com | :white_check_mark:
221 | iddb.isipp.com | :white_check_mark:
222 | images.rbl.msrbl.net | :white_check_mark:
223 | in.dnsbl.org | :white_check_mark:
224 | inputs.orbz.org | :white_check_mark:
225 | intercept.datapacket.net | :white_check_mark:
226 | intruders.docs.uu.se | :white_check_mark:
227 | invalidipwhois.dnsiplists.completewhois.com | :white_check_mark:
228 | ipbl.zeustracker.abuse.ch | :white_check_mark:
229 | ips.backscatterer.org | :white_check_mark:
230 | ipv6.all.dnsbl.bit.nl | :white_check_mark:
231 | ipv6.all.s5h.net | :white_check_mark:
232 | ipwhois.rfc-ignorant.org | :white_check_mark:
233 | ispmx.pofon.foobar.hu | :white_check_mark:
234 | isps.spamblocked.com | :white_check_mark:
235 | is-tor.kewlio.net.uk | :white_check_mark:
236 | ix.dnsbl.manitu.net | :white_check_mark:
237 | korea.rominet.net | :white_check_mark:
238 | korea.services.net | :white_check_mark:
239 | l1.apews.org | :white_check_mark:
240 | l1.apews.rhsbl.sorbs.net | :white_check_mark:
241 | l1.bbfh.ext.sorbs.net | :white_check_mark:
242 | l1.spews.dnsbl.sorbs.net | :white_check_mark:
243 | l2.apews.dnsbl.sorbs.net | :white_check_mark:
244 | l2.bbfh.ext.sorbs.net | :white_check_mark:
245 | l2.spews.dnsbl.sorbs.net | :white_check_mark:
246 | l3.bbfh.ext.sorbs.net | :white_check_mark:
247 | l4.bbfh.ext.sorbs.net | :white_check_mark:
248 | lacnic-main.bogons.dnsiplists.completewhois.com | :white_check_mark:
249 | lacnic.spamblocked.com | :white_check_mark:
250 | lame.dnsbl.rangers.eu.org | :white_check_mark:
251 | lbl.lagengymnastik.dk | :white_check_mark:
252 | list.anonwhois.net | :white_check_mark:
253 | list.bbfh.org | :white_check_mark:
254 | list.blogspambl.com | :white_check_mark:
255 | list.dnswl.org | :white_check_mark:
256 | list.quorum.to | :white_check_mark:
257 | mail-abuse.blacklist.jippg.org | :white_check_mark:
258 | mail.people.it | :white_check_mark:
259 | manual.orbz.gst-group.co.uk | :white_check_mark:
260 | misc.dnsbl.sorbs.net | :white_check_mark:
261 | mr-out.imrss.org | :white_check_mark:
262 | msgid.bl.gweep.ca | :white_check_mark:
263 | mtawlrev.dnsbl.rediris.es | :white_check_mark:
264 | multi.surbl.org | :white_check_mark:
265 | multi.uribl.com | :white_check_mark:
266 | netblockbl.spamgrouper.com | :white_check_mark:
267 | netblockbl.spamgrouper.to | :white_check_mark:
268 | netblock.pedantic.org | :white_check_mark:
269 | netbl.spameatingmonkey.net | :white_check_mark:
270 | netscan.rbl.blockedservers.com | :white_check_mark:
271 | new.dnsbl.sorbs.net | :white_check_mark:
272 | new.spam.dnsbl.sorbs.net | :white_check_mark:
273 | nml.mail-abuse.org | :white_check_mark:
274 | nobl.junkemailfilter.com | :white_check_mark:
275 | nomail.rhsbl.sorbs.net | :white_check_mark:
276 | no-more-funn.moensted.dk | :white_check_mark:
277 | noptr.spamrats.com | :white_check_mark:
278 | noservers.dnsbl.sorbs.net | :white_check_mark:
279 | nospam.ant.pl | :white_check_mark:
280 | nsbl.fmb.la | :white_check_mark:
281 | old.dnsbl.sorbs.net | :white_check_mark:
282 | old.spam.dnsbl.sorbs.net | :white_check_mark:
283 | opm.tornevall.org | :white_check_mark:
284 | orbs.dorkslayers.com | :white_check_mark:
285 | orbz.gst-group.co.uk | :white_check_mark:
286 | origin6.asn.cymru.com | :white_check_mark:
287 | origin.asn.cymru.com | :white_check_mark:
288 | origin.asn.spameatingmonkey.net | :white_check_mark:
289 | orvedb.aupads.org | :white_check_mark:
290 | outputs.orbz.org | :white_check_mark:
291 | pacbelldsl.compu.net | :white_check_mark:
292 | Paidaccessviarsync | :white_check_mark:
293 | pdl.bl.reynolds.net.au | :white_check_mark:
294 | peer.asn.cymru.com | :white_check_mark:
295 | phishing.rbl.msrbl.net | :white_check_mark:
296 | plus.bondedsender.org | :white_check_mark:
297 | pm0-no-more.compu.net | :white_check_mark:
298 | pofon.foobar.hu | :white_check_mark:
299 | policy.lbl.lagengymnastik.dk | :white_check_mark:
300 | postmaster.rfc-clueless.org | :white_check_mark:
301 | postmaster.rfc-ignorant.org | :white_check_mark:
302 | ppbl.beat.st | :white_check_mark:
303 | probes.dnsbl.net.auproxy.bl.gweep.ca | :white_check_mark:
304 | problems.dnsbl.sorbs.net | :white_check_mark:
305 | proxies.blackholes.easynet.nl | :white_check_mark:
306 | proxies.dnsbl.sorbs.net | :white_check_mark:
307 | proxies.exsilia.net | :white_check_mark:
308 | proxies.relays.monkeys.com | :white_check_mark:
309 | proxy.bl.gweep.ca | :white_check_mark:
310 | proxy.block.transip.nl | :white_check_mark:
311 | proxy.drbl.be.net.ru | :white_check_mark:
312 | psbl.surriel.com | :white_check_mark:
313 | pss.spambusters.org.ar | :white_check_mark:
314 | q.mail-abuse.com | :white_check_mark:
315 | query.bondedsender.org | :white_check_mark:
316 | query.senderbase.org | :white_check_mark:
317 | rabl.nuclearelephant.com | :white_check_mark:
318 | random.bl.gweep.ca | :white_check_mark:
319 | rbl2.triumf.ca | :white_check_mark:
320 | rbl.abuse.ro | :white_check_mark:
321 | rbl.atlbl.net | :white_check_mark:
322 | rbl.blakjak.net | :white_check_mark:
323 | rbl.blockedservers.com | :white_check_mark:
324 | rbl.bulkfeeds.jp | :white_check_mark:
325 | rbl.cbn.net.id | :white_check_mark:
326 | rbl.dns-servicios.com | :white_check_mark:
327 | rbl.echelon.pl | :white_check_mark:
328 | rbl.efnethelp.net | :white_check_mark:
329 | rbl.efnet.org | :white_check_mark:
330 | rbl.efnetrbl.org | :white_check_mark:
331 | rbl.eznettools.com | :white_check_mark:
332 | rbl.fasthosts.co.uk | :white_check_mark:
333 | rbl.firstbase.com | :white_check_mark:
334 | rbl.init1.nl | :white_check_mark:
335 | rbl.interserver.net | :white_check_mark:
336 | rbl.ipv6wl.eu | :white_check_mark:
337 | rbl.jp | :white_check_mark:
338 | rbl.lugh.ch | :white_check_mark:
339 | rbl.mail-abuse.org | :white_check_mark:
340 | rbl.ma.krakow.pl | :white_check_mark:
341 | rbl.megarbl.net | :white_check_mark:
342 | rbl.ntvinet.net | :white_check_mark:
343 | rbl.pil.dk | :white_check_mark:
344 | rbl.polarcomm.net | :white_check_mark:
345 | rbl.rope.net | :white_check_mark:
346 | rbl.schulte.org | :white_check_mark:
347 | rbl.snark.net | :white_check_mark:
348 | rbl.spamlab.com | :white_check_mark:
349 | rbl.suresupport.com | :white_check_mark:
350 | rbl.talkactive.net | :white_check_mark:
351 | rbl.triumf.ca | :white_check_mark:
352 | rdts.bl.reynolds.net.au | :white_check_mark:
353 | recent.dnsbl.sorbs.net | :white_check_mark:
354 | recent.spam.dnsbl.sorbs.net | :white_check_mark:
355 | relayips.rbl.shub-inter.net | :white_check_mark:
356 | relays.bl.gweep.ca | :white_check_mark:
357 | relays.bl.kundenserver.de | :white_check_mark:
358 | relays.dnsbl.sorbs.net | :white_check_mark:
359 | relays.dorkslayers.com | :white_check_mark:
360 | relays.nether.net | :white_check_mark:
361 | relays.radparker.com | :white_check_mark:
362 | relays.sandes.dk | :white_check_mark:
363 | relaywatcher.n13mbl.com | :white_check_mark:
364 | rep.mailspike.net | :white_check_mark:
365 | reputation-domain.rbl.scrolloutf1.com | :white_check_mark:
366 | reputation-ip.rbl.scrolloutf1.com | :white_check_mark:
367 | reputation-ns.rbl.scrolloutf1.com | :white_check_mark:
368 | residential.block.transip.nl | :white_check_mark:
369 | rf.senderbase.org | :white_check_mark:
370 | rhsbl.rymsho.ru | :white_check_mark:
371 | rhsbl.scientificspam.net | :white_check_mark:
372 | rhsbl.sorbs.net | :white_check_mark:
373 | rhsbl.zapbl.net | :white_check_mark:
374 | ripe-main.bogons.dnsiplists.completewhois.com | :white_check_mark:
375 | r.mail-abuse.com | :white_check_mark:
376 | rsbl.aupads.org | :white_check_mark:
377 | sa-accredit.habeas.com | :white_check_mark:
378 | safe.dnsbl.sorbs.net | :white_check_mark:
379 | sa.senderbase.org | :white_check_mark:
380 | sbl.nszones.com | :white_check_mark:
381 | schizo-bl.kundenserver.de | :white_check_mark:
382 | score.senderscore.com | :white_check_mark:
383 | sender-address.sjesl.monkeys.com | :white_check_mark:
384 | sender-domain.sjesl.monkeys.com | :white_check_mark:
385 | sender-domain-validate.sjesl.monkeys.com | :white_check_mark:
386 | short.fmb.la | :white_check_mark:
387 | short.rbl.jp | :white_check_mark:
388 | singlebl.spamgrouper.com | :white_check_mark:
389 | singular.ttk.pte.hu | :white_check_mark:
390 | smtp.dnsbl.sorbs.net | :white_check_mark:
391 | socks.dnsbl.sorbs.net | :white_check_mark:
392 | sohul.habeas.com | :white_check_mark:
393 | spam.abuse.ch | :white_check_mark:
394 | spamblock.kundenserver.de | :white_check_mark:
395 | spambot.bls.digibase.ca | :white_check_mark:
396 | spam.dnsbl.anonmails.de | :white_check_mark:
397 | spam.dnsbl.rangers.eu.org | :white_check_mark:
398 | spam.dnsbl.sorbs.net | :white_check_mark:
399 | spamdomain.block.transip.nl | :white_check_mark:
400 | spamdomains.blackholes.easynet.nl | :white_check_mark:
401 | spam.exsilia.net | :white_check_mark:
402 | spamguard.leadmon.net | :white_check_mark:
403 | spamips.rbl.shub-inter.net | :white_check_mark:
404 | spam.lbl.lagengymnastik.dk | :white_check_mark:
405 | spamlist.or.kr | :white_check_mark:
406 | spam.olsentech.net | :white_check_mark:
407 | spam.pedantic.org | :white_check_mark:
408 | spam.rbl.blockedservers.com | :white_check_mark:
409 | spamrbl.imp.ch | :white_check_mark:
410 | spam.rbl.msrbl.net | :white_check_mark:
411 | spam.shri.net | :white_check_mark:
412 | spamsource.block.transip.nl | :white_check_mark:
413 | spamsources.fabel.dk | :white_check_mark:
414 | spamsources.spamblocked.com | :white_check_mark:
415 | spam.spamrats.com | :white_check_mark:
416 | spamsupport.dnsbl.rangers.eu.org | :white_check_mark:
417 | spam.wonk.org | :white_check_mark:
418 | spam.wytnij.to | :white_check_mark:
419 | spam.zapjunk.com | :white_check_mark:
420 | spbl.bl.winbots.org | :white_check_mark:
421 | spews.block.transip.nl | :white_check_mark:
422 | srnblack.surgate.net | :white_check_mark:
423 | srn.surgate.net | :white_check_mark:
424 | stabl.rbl.webiron.net | :white_check_mark:
425 | stale.dict.rbl.arix.com | :white_check_mark:
426 | stale.sa_slip.arix.com | :white_check_mark:
427 | st.technovision.dk | :white_check_mark:
428 | superblock.ascams.com | :white_check_mark:
429 | taiwan.rominet.net | :white_check_mark:
430 | tor.dan.me.uk | :white_check_mark:
431 | tor.dnsbl.sectoor.de | :white_check_mark:
432 | tor.efnet.org | :white_check_mark:
433 | torexit.dan.me.uk | :white_check_mark:
434 | torserver.tor.dnsbl.sectoor.de | :white_check_mark:
435 | truncate.gbudb.net | :white_check_mark:
436 | trusted.nether.net | :white_check_mark:
437 | ubl.lashback.com | :white_check_mark:
438 | ubl.nszones.com | :white_check_mark:
439 | ubl.unsubscore.com | :white_check_mark:
440 | unsure.nether.net | :white_check_mark:
441 | uribl.abuse.ro | :white_check_mark:
442 | uribl.pofon.foobar.hu | :white_check_mark:
443 | uribl.spameatingmonkey.net | :white_check_mark:
444 | uribl.swinog.ch | :white_check_mark:
445 | uribl.zeustracker.abuse.ch | :white_check_mark:
446 | urired.spameatingmonkey.net | :white_check_mark:
447 | url.rbl.jp | :white_check_mark:
448 | v4.fullbogons.cymru.com | :white_check_mark:
449 | v6.fullbogons.cymru.com | :white_check_mark:
450 | vbl.mookystick.com | :white_check_mark:
451 | virbl.bit.nl | :white_check_mark:
452 | virbl.dnsbl.bit.nl | :white_check_mark:
453 | virus.rbl.jp | :white_check_mark:
454 | virus.rbl.msrbl.net | :white_check_mark:
455 | vote.drbl.be.net.ru | :white_check_mark:
456 | vote.drbl.caravan.ru | :white_check_mark:
457 | vote.drbl.croco.net | :white_check_mark:
458 | vote.drbl.dataforce.net | :white_check_mark:
459 | vote.drbldf.dsbl.ru | :white_check_mark:
460 | vote.drbl.gremlin.ru | :white_check_mark:
461 | vote.drbl.host.kz | :white_check_mark:
462 | vote.rbl.ntvinet.net | :white_check_mark:
463 | vouch.dwl.spamhaus.org | :white_check_mark:
464 | wadb.isipp.com | :white_check_mark:
465 | wbl.triumf.ca | :white_check_mark:
466 | wdl.bl.reynolds.net.au | :white_check_mark:
467 | web.dnsbl.sorbs.net | :white_check_mark:
468 | web.rbl.msrbl.net | :white_check_mark:
469 | whois.rfc-clueless.org | :white_check_mark:
470 | whois.rfc-ignorant.org | :white_check_mark:
471 | wl.mailspike.net | :white_check_mark:
472 | wl.nszones.com | :white_check_mark:
473 | wl.summersault.com | :white_check_mark:
474 | wl.trusted-forwarder.org | :white_check_mark:
475 | work.drbl.caravan.ru | :white_check_mark:
476 | work.drbl.croco.net | :white_check_mark:
477 | work.drbl.dataforce.net | :white_check_mark:
478 | work.drbldf.dsbl.ru | :white_check_mark:
479 | work.drbl.gremlin.ru | :white_check_mark:
480 | work.drbl.host.kz | :white_check_mark:
481 | worm.dnsbl.rangers.eu.org | :white_check_mark:
482 | wormrbl.imp.ch | :white_check_mark:
483 | worms-bl.kundenserver.de | :white_check_mark:
484 | wpb.bl.reynolds.net.au | :white_check_mark:
485 | xbl.selwerd.cx | :white_check_mark:
486 | ybl.megacity.org | :white_check_mark:
487 | zebl.zoneedit.com | :white_check_mark:
488 | zen.spamhaus.org | :white_check_mark:
489 | z.mailspike.net | :white_check_mark:
490 | z.mailspike.net | :white_check_mark:
491 | zombie.dnsbl.sorbs.net | :white_check_mark:
492 | zta.birdsong.org | :white_check_mark:
493 | ztl.dorkslayers.com | :white_check_mark:
494 | zz.countries.nerd.dk | :white_check_mark:

Many of them support IPv6. To be continued in another project.


## Sources
* http://multirbl.valli.org/list/
* http://download.xskernel.org/howto/mail/filter-dnsbl-lists.htm
* http://www.psicopolis.com/webmasters/ektorgeorgiakis/clean/spamlinks.html/filter-dnsbl.htm
* https://en.wikipedia.org/wiki/DNSBL
* https://en.wikipedia.org/wiki/Comparison_of_DNS_blacklists
* http://ipindetail.com/ip-blacklist-checker
* http://rblcheck.at/

