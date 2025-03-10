- DNS -- domain name system, система, которая переводит текстовый адрес в IP адрес.
- Преобразование текстового адреса в ip называется resolution.
- ## 4 DNS сервера:
  heading:: 2
	- DNS recursor -- как библиотекарь, которого попросили найти нужную книгу. Получает запросы от клиентов, может делать запросы к другим DNS серверам, чтобы найти нужный адрес. 
	  logseq.order-list-type:: number
	- Root nameserver -- первый шаг в переводе адреса. Ищет подходящую "стойку с книгами".
	  logseq.order-list-type:: number
	- TLD nameserver -- указывает на адреса внутри определенного верхнеуровневого домена, типа .com.
	  logseq.order-list-type:: number
	- Authoritative nameserver -- указывает уже на конкретный адрес. Найденный адрес возвращается DNS рекурсору. 
	  logseq.order-list-type:: number
	- Если есть "поддомен", то есть лейбл выше 1 уровня, то добавляются дополнительные nameservers. [Тут](https://www.cloudflare.com/ru-ru/learning/dns/dns-records/dns-cname-record/) можно почитать подробнее.
	  logseq.order-list-type:: number
- ## Шаги разрешения DNS
  heading:: 2
	- A user types ‘example.com’ into a web browser and the query travels into the Internet and is received by a DNS recursive resolver.
	  logseq.order-list-type:: number
	- The resolver then queries a DNS root nameserver (.).
	  logseq.order-list-type:: number
	- The root server then responds to the resolver with the address of a Top Level Domain (TLD) DNS server (such as .com or .net), which stores the information for its domains. When searching for example.com, our request is pointed toward the .com TLD.
	  logseq.order-list-type:: number
	- The resolver then makes a request to the .com TLD.
	  logseq.order-list-type:: number
	- The TLD server then responds with the IP address of the domain’s nameserver, example.com.
	  logseq.order-list-type:: number
	- Lastly, the recursive resolver sends a query to the domain’s nameserver.
	  logseq.order-list-type:: number
	- The IP address for example.com is then returned to the resolver from the nameserver.
	  logseq.order-list-type:: number
	- The DNS resolver then responds to the web browser with the IP address of the domain requested initially.
	  logseq.order-list-type:: number
	- ![Complete DNS Lookup and Webpage Query - 10 steps](https://cf-assets.www.cloudflare.com/slt3lc6tev37/1NzaAqpEFGjqTZPAS02oNv/bf7b3f305d9c35bde5c5b93a519ba6d5/what_is_a_dns_server_dns_lookup.png)
- ## Прочее
  heading:: 2
	- Есть DNS кэшинг, в том числе локальный.
- ## Ресурсы
  heading:: 2
	- Прикольная штука для экспериментов и полезных опытов: [тык](https://messwithdns.net/)