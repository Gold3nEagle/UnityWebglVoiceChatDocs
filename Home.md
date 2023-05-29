# Unity Webgl Voice Chat Solution
Welcome to the Docs of the Webgl Voice Chat Solution

this Documentation is meant to explain the mechanism of how does the software work, document it's functions and help you getting started.


to get started check the getting started page.


***


to learn more about the functions and functionality of the solution check the docs section

# How it works

in simple terms, the voice chat is a separate application that runs on your browser. the twist is that scripts were written in unity, allowing communication between unity and the voice chat application itself (the js file from the server).

so unity communicates with it while the application handles rooms, and people, and fetches the voice from the microphone with transmission over the network, etc... .

the reason it was built this way is that the in-availability of access to the mic in unity for WebGL, alongside the limitation of only TCP/HTTP protocol on the browser prevents the development of a functional voice chat within the unity game on the browser and thus the current solution.