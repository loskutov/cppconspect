STL
===

Идея parallel_reduce

Состоит из:

* #### Containers
  - sequences
    + vector
    + list
    + deque
  - associative
    + [unordered_][multi][set, map]

* #### Algorithms
  + find
  + find_if
  + sort *(qsort)*
  + stable_sort *(merge sort)*
  + remove
  + remove_if
  + search
  + next_permutation
  + prev_permutation
  + unique
  + reverse
  + lower_bound
  + upper_bound
  + nth_element
  + random_shuffle

* #### Iterators
    + push_back
    + pop_back
    + back

Signals
=======

    struct timer {
        timer(function<void ()> on_tick) {
            on_tick(on_tick);
        }
        ...
        on_tick();
    }
    vector<function<void ()>> -- signal

    struct signal {
        typedef function<void()> slot;
        signal() {};
        struct connection;
        void connect(slot s) {
            slots.push_back(s);
        }
        void operator()() const {
            for(auto const& s : slot)
                s();
        }
        private:
        vector<slot> slots;
    }
    struct signal::connection {
        void disconnect()
    }


----------

* function
* signal

---------------------
---------------------

IP-address
192.168.3.118

0.0.0.0 -- any
255.255.255.255 -- broadcast
10.0.0.0 -- 10.255.255.255      -- 24 bit

192.168.0.0. -- 192.168.255.255 -- 16 bit

172.16.0.0 -- 172.31.255.255    -- 20bit

127.0.0.0 -- 127.255.255.255    -- localhost

ICMP -- Internet Control Message Protocol

TCP -- Transmission Control Protocol
    (connection-oriented)
UDP -- User Datagram Protocol
65536 ports

MTU = maximal transfer unit -- 1500 bytes

BSD-socket
UNIX domain socket

server                         |        client
-------------------------------|------------------------
s = socket(...SOCK_STREAM)     |      r = socket(...SOCK_STREAM)
bind(s, local_addr)            |      connect(s,...,remote_addr)
s1 = accept(s);                |    
recv()                         |        recv(s)
send()                         |        send(s)
close(s1)                      |        close(r);
close(s)                       |  



UDP
===

server                                  client
s = socket(...SOCK_DGRAM)            r = socket(...SOCK_DGRAM)
bind(s, local_addr)                   connect(s,...,remote_addr)
s1 = accept(s);
recv()                                  recv(s)
send()                                  send(s)
close(s1)                               close(r);
close(s)



+ select -- bit mask, size=1024
+ poll -- dynamic array of file descriptors
+ epoll_ctl
+ epoll_wait
+ epoll_wait
+ epoll_wait


    token = async_wait(io_service&, int, function)

+ timerfd
+ eventfd
+ signalfd -- ваще норм тема))
+ file

HTTP
====

Spring 1992

    GET /url HTTP/1.0
    key: value
    key: value

    //response
    key: value
    key: value

    DATA



    POST
    PUT
    DELETE

* http-proxy (cache)
* wget
* pastebin/wiki
* chat

------------------

* QTcpServer
* QTcpSocket
* QHttp (old)
* QNetworkManager (new)


Type erasure
============

A | B | C
--|---|--
f | f | f

X f

    any_iterator <bidirectional_iterator_blabla> a = v.begin();
    any<f, g, h> f;

    struct any {
        struct interface {
            virtual ~interface();
        };
        template<typename T>
        struct impl : interface {
            static char* get_static_tag();
            get_tag();
            T data;
        };
        any(T a) : p(new impl<T>(a)) {}
        T* any_cast() {
            if(auto * q = dynamic_cast<impl<T>>)
                p.get();
        }

        char* any::impl<T>::get_static_tag() {
            static char tag;
            return &tag;
        }

        char * any::impl<T>::get_tag() {
            return get_static_tag();
        }

        private:
            unique_ptr<interface> p;
    }

RTTI
====
RunTime Type Information


Variadic templates
==================

perfect forwarding


    struct optional {
        template<typename InPlace>
        optional& operator=(InPlace inplace) {
            inplace.create(data);
            initialized = true;
        // тут я, кажется, заснул :/
        }
    }
