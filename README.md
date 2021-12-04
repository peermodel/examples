# examples

First, please download the jar file of the Peer-Model translator and follow the instructions in the readme file  https://github.com/peermodel/translator#readme there.

Tutorial on how to model a use case and how to use the toolchain: https://youtu.be/SkuHuomUCtY


<hr>

<h2>Open, translate and run an existing use case with Draw.io</h2>

<h3>Open https://www.draw.io in a browser</h3>

<h3>Open existing diagram</h3>

Let <em>LDIR</em> be the directory where you checked out the examples found in https://github.com/peermodel/examples.

Open ClientServer.xml found in <em>LDIR</em>/examples/_USE-CASES/_DRAWIO/Apps/.

The ClientServer use case models two peers: A client peer and a server peer. The client peer sends exactly one request to the server peer, when it is started and waits for an answer to its request. The requests contains the ID of the client peer so that the server knows where to send the answer. The server peer waits for a request and sends the answer to the peer who sent the request. The answer contains the ID of the server peer so that the client knows who processed its request. Both answer and entry message are modeled with entries.

The ClientServer diagram has 4 drawings:

<strong>Config: One</strong>: This diagram contains a configuration of the use case termed &ldquo;One&rdquo; with one peer instance named superServer of type Server, and one peer instance named client1 of type Client. The peer property (pprop) server of client1 is configured with superServer. Note there can be many configurations contained in a use case diagram. The name of a drawing that contains a configuration must start with &ldquo;Config:&rdquo;. 

<strong>EntryTypes</strong>: This diagram contains the needed entry types termed request and answer. Request has an entry property (eprop) named client (holding the URL of a client peer). Answer has two properties termed server (holding the URL of a server peer) and answer (holding the reply message string). 

<strong>PeerType: Server</strong>: This diagram contains the Peer Model of the server peer. First the peer type must be specified which is termed Server. It has one wiring referred to as reply. Guard 1 of the wiring waits for an answer entry, takes it from the peer's PIC (peer-input-container) and remembers the value of its client property in a local variable termed $client. Action 1 of the wiring creates an answer entry, sets its answer property to &ldquo;answer from this server&rdquo; and sends it back to the client whose URL was found in the local variable. Note that asynchronous sending is done by setting the link property (lprop) named dest (destination) to the URL of the peer to whom the entries collected along the action link shall be sent. This causes the entries to be written in the PIC of the system peer termed I/O which in turn will (asynchronously) treat these entries and write them to the PIC of the peer denoted in the dest property.

<strong>PeerType: Client</strong>: This diagram contains the client peer type. Its name is Client and it has one property declaration, namely server of type URL. This property is set in the configuration to the actual name of the server peer, which in our case is called superServer. The rest of this drawing models the behavior of the client peer with two wirings called start and stop. Guard 1 of the start wiring removes the system entry INIT (which is created by the system in a peer's PIC if the peer is instantiated) from the peer's PIC, and action 1 creates a request entry, sets its client property to the own peer ID, and sets the action link's dest property to the server peer configured in the client peer's server property. Note that own peer properties can be accessed within a peer with the qualifier &ldquo;PEER&rdquo;. Note that $$PID is a system variable that holds the own peer's ID, which is of type URL. Guard 1 of the stop wiring takes an answer entry from the POC (note that the link is blocked until the response is there) and writes it in action 2 to the POC (peer-output-container); in addition, it creates a STOP entry and sends it to the Stop peer (action 2). Note that STOP is a system-defined entry type and Stop is a system peer that upon receipt of a STOP entry shut down the entire system.

<h3>Store the use case as not-compressed xml</h3>

Make sure that File -> Properties Compressed is not set!

Save file as XML with a name that you may select, or just overwrite the existing file.


<h3>Translate and run the use case</h3>

See https://github.com/peermodel/translator/blob/main/README.md.

The result of the simulation run will show the state of space after the run has terminated, namely all PIC and POC containers of peers that are not empty. In our case you will see the POC of client1 peer containing the received answer entry whose property answer is set to the message received from the superServer peer.



