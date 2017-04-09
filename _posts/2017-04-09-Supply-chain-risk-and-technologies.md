---
layout: posts
excerpt_separator: <!--more -->
title: Supply Chain Risk and Technologies
comments: False
---
Managing a supply chain invariably means managing various processes, operations and systems, all of which have associated financial risks. <!--more -->Some of these risks have different probabilities of occurring. If problems occur, their impact on the supply chain and operation can vary.

For instance, inventory shortages or excess have a relatively high probability of occurring in most operations. Poor forecasting can cause this predicament but so too can the mismanagement of new product introduction (NPI) or end of life (EOL) run down of stock. Arguably, inventory shortages or excess have little impact on an operation in comparison to say a natural disaster that cuts off the supply of a required resource. The devastating flood in Thailand in 2011, resulted in an unfortunate loss of life. Another effect was a worldwide shortage of PC [hard drives](http://www.anandtech.com/show/5127/thailands-flooding-and-our-hdd-needs).

When thinking about the features for supplychainpy (my supply chain analytics focused python library side project), it was helpful for me to refer to the matrix below. I have cherry-picked some of the most generic but important risks.


<table>
	<tr>
        <td rowspan="2"><strong>Impact</td>
        <td style="text-align:center;" colspan="2"> <strong >Probability </strong></td>
    </tr>
    <tr>
        <td style="text-align:center;" ><strong>Low</strong></td>
        <td><strong>High</strong></td>
    </tr>
    <tr>
        <td><strong>High</strong></td>
        <td>
            <ul>
                <li>Natural Disaster</li>
                <li>Multi Port Closure</li>
                <li>Contamination</li>
                <li>Slave Labour</li>
                <li>Production and Order Fulfilment Mis-match</li>
                <li>Supplier Bankrupcy</li>
                <li>Incorrect Liscensing</li>   
            </ul>
        </td>
        <td>    
            <ul>
                <li>Poor New Product Introduction</li>
                <li>Poor end of life planning</li>
                <li>Incorrect Supplier Relationship (e.g. contratual SLA)</li>
                <li>Duties and Taxes</li>
                <li>Data inaccuracy</li>   
                <li>Skills Gap</li>                                                                                                                                                                                                                                                                                   
            </ul>
        </td>
    </tr>
    <tr>
        <td><strong>Low</strong></td>
        <td>
            <ul>
                <li>Shrinkage (theft)</li>
                <li>Obsolesence</li>
                <li>Equipment Failure</li>
                <li>Quality</li>
            </ul>
        </td>
        <td>
            <ul>
               <li>Inventory (Excess/Shortage)</li>
               <li>Transportation Disruption</li>
               <li>Single Port Closure</li>
            </ul>
        </td>
    </tr>
</table>


The location of items in this model may vary depending on the size of the organisation, their industry and their position within their network of suppliers and geographic location. 

## Technologies that might help

There are several technologies I keep an eye on because a solution implementing one or two of them may just help mitigate some of these risks. The technologies mentioned below are also likely to feature heavily in the supply chain.

### Probabilistic databases

My first pick is left field and not "hyped" by the media yet. Probabilistic databases are still under heavy development. As far as I am aware, a commercial platform does not exist. Probabilistic databases hope to solve the problem of missing data in a database or across related but disparate databases without a common primary key. Using weights for the various possible "worlds" (versions of tuples) the database can serve up several different answers with varying probabilities of being present in the database. I'm not even sure I understand them...but I want to believe!

<table>
    <tr>
        <td><strong>Risks Mitigated</strong></td>
        <td>
           <ul>
            <li>Data inaccuracy</li>
            <li>New product introduction (NPI)</li>
            </ul> 
        </td>
   </tr>
   <tr>
           <td><strong>Companies/Tools</strong></td>
        <td></td>
   </tr>
</table>

### Blockchain

The benefit of blockchain for supply chain management is a hot topic. I won't lie, I briefly wondered how I could fit blockchain into supplychainpy. Thankfully, I have my hands full implementing and refactoring the bread and butter of the library. I am still curious about its possible application.

The blockchain is an open distributed ledger. Peer-to-peer database maintains accurate transaction records autonomously. The technology is a core component of [bitcoin](https://en.wikipedia.org/wiki/Bitcoin) (digital currency). 

There seems to be a gold rush in supply chain management centred around this new technology. Many startups and established companies are focusing on the Blockchain. 

Real-time translation of analogue to digital in supply chains has always been hard. Deriving insight from the process has been even harder.

Warehousing, trunking and break bulk activities, may be the first beneficiaries. Having a real-time ledger that reconciles every inventory movement across all warehouses in a big operation would be a big win.

<table>
	<tr>
        <td><strong>Risks Mitigated</strong></td>
        <td>
            <ul>
                <li> Shrinkage </li>
                <li> Slave labour </li>
        </ul> 
    </td>
    </tr>
    <tr>
        <td><strong>Companies/Tools</strong></td>
        <td>
            <ul>
                <li><a href="https://www.provenance.org/">Provance</a></li>
                <li><a href="https://www.skuchain.com/">Skuchain</a></li>
            </ul> 
        </td>
    </tr>
</table>


### RFID

An oldy but a goody, these tags use radio frequency to communicate with a reader. They are often used in conjunction with some asset management software like a warehouse management system (WMS) or something. RFIDs can be active passive and replace line-of-sight labelling like barcodes. The passive and active variants differ in signal range and cost. The difference in signal range and price have implications for when and where their use is cost-effective. The last interesting thing I remember reading about RFIDs was research regarding the development of bio-degradable tags. 

<table>
	<tr>
        <td><strong>Risks Mitigated</strong></td>
        <td>
            <ul>
                <li> Shrinkage </li>
            </ul> 
        </td>
    </tr>
    <tr>
        <td><strong>Companies/Tools</strong></td>
        <td>
            <ul>
                <li>loads</li>
            </ul> 
        </td>
    </tr>
</table>


### Bluetooth Piconets

Another technology that doesn't get as much press as it used to but had industrial applications in farming and marketing from what I remember. Piconets are small ad hoc networks of Bluetooth devices. I still think the best is yet to come for this technology.

<table>
    <tr>
        <td><strong>Risks Mitigated</strong></td>
        <td>
            <ul>
                <li> Not really sure </li>
            </ul> 
        </td>
    </tr>
    <tr>
        <td><strong>Companies/Tools</strong></td>
        <td>
            <ul>
                <li>?</li>
            </ul> 
        </td>
    </tr>
</table>


### Telematics

Vehicle telematics was once big news. However, I think the use of GPS and vehicle telemetry is pretty common in many logistics operations. Still, it is an important technology that is under continuous innovation, and now that autonomous vehicles are going to be a thing, telematics will probably be big news again.

<table>
	<tr>
        <td><strong>Risks Mitigated</strong></td>
        <td>
            <ul>
                <li> Oil prices </li>
                <li> Driving Regulations </li>
            </ul> 
        </td>
    </tr>
    <tr>
        <td><strong>Companies/Tools</strong></td>
        <td>
            <ul>
                <li>loads</li>
            </ul> 
        </td>
    </tr>
</table>


### Autonomous Vehicles

Never thought I'd live to see this but autonomous lorries trunking between distribution centres is probably going to happen soonish. One day they are likely to be doing the last mile of deliveries too, although for some reason drones have been seeing the most investment and attention for this application.

<table>
	<tr>
		<td><strong>Risks Mitigated</strong></td>
		<td>
			<ul>
				<li> Oil prices </li>
                <li> Driving Regulations </li>
            </ul> 
        </td>
    </tr>
    <tr>
        <td><strong>Companies/Tools</strong></td>
        <td>
            <ul>
                <li>loads</li>
            </ul> 
        </td>
    </tr>
</table>


### Drones

Drones are an interesting one, autonomous flying machines that promise to deliver to your doorstep (the last mile in industry speak). I have been sceptical because I think the payload is prohibitive. However, I guess if you had a manned centralised distribution vehicle and launched drones from say the back of an arctic or something to service a particular radius then this might work. The operator could keep sending the drones out until the area was served and drive to the next; a fleet of these guys sounds more plausible to me. Regardless of what I think, investment in drones is a thing thanks, to Amazon and the like.

<table>
	<tr>
        <td><strong>Risks Mitigated</strong></td>
        <td>
            <ul>
                <li>NA</li>
            </ul> 
        </td>
    </tr>
    <tr>
        <td><strong>Companies/Tools</strong></td>
        <td>
            <ul>
                <li>Amazon</li>
            </ul> 
        </td>
    </tr>
</table>


### Additive Manufacturing (3D printing)

Additive manufacturing was supposed to have revolutionised product development overnight. It was speculated that the technology would do to the physical widget, what the internet did to journalism, music and film. It still might. It is used in industrial settings and by enthusiast, and prosumers. I'm getting my very own 3d printer this year (still saving up the pennies).


<table>
	<tr>
        <td><strong>Risks Mitigated</strong></td>
        <td>
            <ul>
                <li> Lead time unucertainty </li>
                <li> Inventory excess/shortage</li>
                <li> Quality management </li>
            </ul> 
        </td>
    </tr>
    <tr>
        <td><strong>Companies/Tools</strong></td>
    <td>
        <ul>
            <li><a href="https://voodoomfg.com">Vodoo Manufacturing</a></li>
            <li><a href="https://i.materialise.com">iMaterialise</a></li>
            <li><a href="https://www.shapeways.com">Shapeways</a></li>
        </ul> 
    </td>
   </tr>
</table>


### Augmented Reality

Apparently, Google glass didn't go away; it went to work. Google glass is in factories (yep just burst right in), according to an NPR article I read. I can see this being useful for providing additional information about processes on the shop floor. For example, status updates about the machines being used, production throughput, machine health statistics, etc. Also, application in pick and sort areas. Augmented reality is probably a safe bet, especially with Microsoft Hololens around the corner. 

<table>
	<tr>
        <td><strong>Risks Mitigated</strong></td>
        <td>
            <ul>
                <li> NA</li>         
            </ul> 
        </td>
    </tr>
    <tr>
        <td><strong>Companies/Tools</strong></td>
        <td>
            <ul>
                <li>Google</li>
                <li>Amazon</li>
            </ul> 
        </td>
    </tr>
</table>

### Big Data Analytics and Machine Learning

Big data analytics applications and tools like Apache Spark, Hadoop and SAP Hana, to name a few, are big news. There are so many call for papers and publications on this topic right now in supply chain and operations management and computer science. The development and implementation of big data analytics tools and platforms, machine learning and AI are the new arms race in the industry. I love it! Open source libraries like Tensor flow are making it easier for developers and industry to get their hands dirty. It is tough to implement real-time end-to-end analytics in industry. Some organisation do it well, and others just don't know where to start.

Also, GPGPU processing should get a mention in this space because Nvidia and their GPU lineup for deep learning continue to have an impact on the analytics platforms of the future. Nvidia's big bet on the importance of GPGPU processing and provision of the best tools possible with CUDA have made them the defacto standard. 

Personally, I hope OpenGL gets more support so we can have more choice over the GPUs used, AMD GPUs are often cheaper but not as accessible for building and training model because of the lack of support. There are other alternatives such as Intel's Xeon Phi coprocessors, but they are not as easy to get your hands on and have enterprise prices and support.

<table>
	<tr>
		<td><strong>Risks Mitigated</strong></td>
        <td>
            <ul>
                <li> Skills Gap</li>         
            </ul> 
        </td>
    </tr>
    <tr>
        <td><strong>Companies/Tools</strong></td>
        <td>
            <ul>
                <li>Apache</li>
                <li>SAP</li>
                <li>ORACLE</li>
                <li><a href="http://www.tamr.com">Tamr</a></li>
            </ul> 
        </td>
    </tr>
</table>