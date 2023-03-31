- > 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.mycloudwiki.com](https://www.mycloudwiki.com/san/ipsan-overview/)
  
  > As discussed in previous chapter Fibre Channel (FC) SAN provides high performance and scalability. Th......
  
  As discussed in previous chapter Fibre Channel (FC) SAN provides high performance and scalability. These advantages of FC SAN come with the burden of additional cost of buying FC components, such as FC HBA and FC switches. So to overcome these cost burden, we have another type of Storage Area networks called IP SAN.  
    
  
  IP SAN uses Internet Protocol (IP) for the transport of storage traffic instead of Fibre Channel (FC) cables. It transports block I/O over an IP-based network. Two primary protocols that leverage IP as the transport mechanism for block-level data transmission are 
  
  *   Internet SCSI (iSCSI)
  
  *   Fibre Channel over IP (FCIP).
  
  iSCSI is a storage networking technology which allows storage resources to be shared over an IP network Whereas FCIP is an IP-based protocol that enables distributed FC SAN islands to be interconnected over an existing IP network. In FCIP, FC frames are encapsulated onto the IP payload and transported over an IP network.
  
  IP is a matured technology and using IP as a storage networking option provides several advantages. 
  
  *   Most organizations have an existing IP-based network infrastructure, which could also be used for storage networking and may be a more economical option than deploying a new FC SAN infrastructure.
  
  *   IP network has no distance limitation, which makes it possible to extend or connect SANs over long distances. With IP SAN, organizations can extend the geographical reach of their storage infrastructure and transfer data that are distributed over wide locations.
  
  *   Many long-distance disaster recovery (DR) solutions are already leveraging IP-based networks. In addition, many robust and mature security options are available for IP networks.
  
  Typically, a storage system comes with both FC and iSCSI ports. This enables both the native iSCSI connectivity and the FC connectivity in the same environment.
  
  Lets look at how iSCSI based SAN works and FC based IP SAN works in this chapter
  
  **iSCSI SAN Overview**
  ----------------------
  
  As said earlier, iSCSI is a storage networking technology which allows storage resources to be shared over an IP network and most of the storage resources which are shared on an iSCSI SAN are disk resources. Just like as SCSI messages are mapped on Fibre Channel in FC SAN, iSCSI is a mapping of SCSI protocol over TCP/IP. 
  
  iSCSI is an acronym for Internet Small Computer System Interface, it deals with block storage and maps SCSI over traditional TCP/IP. This protocol is mostly used for sharing primary storage such as disk drives and in some cases it is used for disk backup environment aswell.  
    
  SCSI commands are encapsulated at each layer of the network stack for eventual transmission over an IP network. The TCP layer takes care of transmission reliability and in-order delivery whereas the IP layer provides routing across the network.  
    
  
  [![](https://i0.wp.com/www.mycloudwiki.com/wp-content/uploads/2016/08/iSCSISAN.jpg?resize=400%2C251)](https://www.mycloudwiki.com/wp-content/uploads/2016/08/iSCSISAN.jpg)
  
  In iSCSI SAN, initiators issue read/write data requests to targets over an IP network. Targets respond to initiators over the same IP network. All iSCSI communications follow this request response mechanism and all requests and responses are passed over the IP network as iSCSI Protocol Data Units (PDUs). iSCSI PDU is the fundamental unit of communication in an iSCSI SAN.  
    
    
  iSCSI performance is influenced by three main components. Best initiator performance can be achieved with dedicated iSCSI HBAs, target performance can be achieved by purpose-built iSCSI arrays and finally the network performance can be achieved by dedicated network switches
  
  Multiple layers of security should be implemented on an iSCSI SAN as security is the most important in IT infra. These include CHAP for authentication, discovery domains to restrict device discovery, network isolation and IPsec for encryption of in-flight data.
### iSCSI components

iSCSI is an IP-based protocol that establishes and manages connections between hosts and storage systems over IP. iSCSI is an encapsulation of SCSI I/O over IP.   

iSCSI encapsulates SCSI commands and data into IP packets and transports them using TCP/IP. iSCSI is widely adopted for transferring SCSI data over IP between hosts and storage systems and among the storage systems. It is relatively inexpensive and easy to implement, especially environments in which an FC SAN does not exist.

[![](https://i0.wp.com/www.mycloudwiki.com/wp-content/uploads/2016/08/1-5.jpg?resize=244%2C320)](https://i0.wp.com/www.mycloudwiki.com/wp-content/uploads/2016/08/1-5.jpg)

Key components for iSCSI communication are

*   iSCSI initiators such as an iSCSI HBA

*   iSCSI targets such as a storage system with an iSCSI port

*   IP-based network such as a Gigabit Ethernet LAN An iSCSI initiator sends commands and associated data to a target and the target returns data and responses to the initiator.

**Go To >>** [Index Page](https://www.mycloudwiki.com/storage-area-network-san-basic-tutorials)
-