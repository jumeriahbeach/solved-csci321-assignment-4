Download Link: https://assignmentchef.com/product/solved-csci321-assignment-4
<br>
In this assignment you have been provided with code that simulates an unreliable link between sender and receiver. This link has a very constrained buffer (only two packets can be ‘in flight’ at a time), and can have arbitrary delay and loss rates. Your job will be to create and implement a protocol over this connection that correctly transfers data, in a reasonable amount of time.

<h1>Implementing Your Solution</h1>

This assignment contains several tools that will help you simulate and test your solution. You should not make changes to any file other than <strong>source.py</strong>. All other files contain code used to either simulate the unreliable connection, or code to help you test your solution.

Your solution file (<strong>source.py</strong>) will be tested against stock versions of all the other files in the folder, so any changes you make will not be present at grading time.

Your solution must be contained in the <strong>send</strong> and <strong>recv</strong> functions in <strong>source.py</strong>. You should not change the signatures of these functions, only their bodies. Your solution must be general, and should work for any file for testing purposes.

Your task is to modify the bodies of these functions so that they communicate using a protocol that ensures that the data sent by the send function can be reliably and quickly reconstructed by the <strong>recv</strong> function. You should do so through a combination of setting timeouts on socket reads (e.x. <strong>socket.timeout(float))</strong> and developing a system through which each side can acknowledge if or when they receive a packet.

Remember that the connection is bandwidth constrained. No more than two packets can be “on the wire” at a time. If you send a third packet while there are already two packets traveling to their destination (in either direction), the third packet will be dropped, so it is important that you get your timeouts and your acknowledgments right.

<h1>Testing Your Solution</h1>

You can use the provided <strong>tester.py</strong> script when testing your solution. This script uses the <strong>receiver.py</strong>, <strong>sender.py</strong>, and <strong>server.py</strong> scripts to simulate an unreliable connection, and to test your solution.

The <strong>tester.py</strong> script contains many parameters you can use to test your solution under different conditions, and to receive different amounts of debugging information to better understand the network. These parameters and options can be viewed by calling <strong>tester.py –-help</strong> and are also reproduced below.

<table width="648">

 <tbody>

  <tr>

   <td width="648">usage: tester.py [-h] [-p PORT] [-l LOSS] [-d DELAY] [-b BUFFER] -f FILE</td>

  </tr>

  <tr>

   <td width="648">                [-r RECEIVE] [-s] [-v]optional arguments:-h, –help            show this help message and exit-p PORT, –port PORT  The port to simulate the lossy wire on (defaults to 9999).-l LOSS, –loss LOSS  The percentage of packets to drop.-d DELAY, –delay DELAY The number of seconds, as a float, to wait before forwarding a packet on.-b BUFFER, –buffer BUFFER The size of the buffer to simulate.-f FILE, –file FILE The file to send over the wire.</td>

  </tr>

  <tr>

   <td width="648">-r RECEIVE, –receive RECEIVE The path to write the received file to. Results are written to a temp file otherwise</td>

  </tr>

  <tr>

   <td width="648">-s, –summary         Print a one line summary instead of the verbose mode output.-v, –verbose         Enable extra verbose mode.</td>

  </tr>

 </tbody>

</table>

<strong> </strong>

For example, to see how your solution performs when transmitting a text file, with a 5% loss rate, and with a

latency of 100ms, you could use the following:




<strong>python3 tester.py –file test_data.txt –loss .05 –delay 0.1. </strong>

<h1>Additional Hints</h1>

<ul>

 <li>A key part of this assignment is determining how long to wait before resending a packet. You should estimate this timeout value using the weighted moving average (discussed in class) technique for estimating the RTT, and use this in determining your timeout. With correctly tuned timeouts, lower RTT will result in higher throughput.</li>

</ul>




<ul>

 <li>A good way of determining the timeout to use is the “<strong>estimated RTT + (deviation of RTT * 4)</strong>”. You should check with your book for more details.</li>

</ul>




<ul>

 <li>Use the included <strong>–verbose</strong> option to include very detailed information about what your code is sending over the network, and how the network is handling that data.</li>

</ul>




<ul>

 <li>Use the included <strong>–receive</strong> option to see the results of your file transfer. By default, the testing script will store the results of your code to a temporary location. This option may be useful if you’re not sure how or why the received file does not match the sent file.</li>

</ul>




<ul>

 <li>Make sure you try your solution under many different loss ratios and latencies by changing the parameters in the tester.py script.</li>

</ul>




<ul>

 <li>Keep your packets smaller than or equal to <strong>MAX_PACKET</strong> (1400 bytes).</li>

</ul>




<ul>

 <li>Pay attention to the end of the connection. Ensure that both sides of the connection finish without user assistance, even if packet losses occur, while guaranteeing that the entire file is transferred. Look at the FIN/FINACK/ACK sequence in TCP for ideas.</li>

</ul>