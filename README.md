Offline Use Cases
================================
There has been a lot of varied work being done within the W3C on offline behaviour. A number of issues were existing with these implementations, so we have collected a set of Use Cases to aid further work. The contributors are a collection of technologists from large telecommunications organisations.

States
------
Throughout we talk about such things as “offline state” and “online state”. Here is a better description of these phrases:

*	OFFLINE STATE: the user has no connectivity to the web.
*	ONLINE STATE: the user can access the web via WiFi or via the operator network.


Organising Use Cases
--------------------

Use cases are organised into the following categories:

* Caching Assets: caching assets for use when offline, can include both "shell" and "content use cases"
* Online / Offline State Change Behaviour: state change events trigger actions to invoke offline / online mechanisms for continued user experience
* Privileges and Priorities: some use cases mean users may need to extend the priveleges they offer to browsers
* "Installed" Web Apps: In this case “Installed” means a web app which is downloaded to the device and coded using web technologies.

Caching Assets Use Cases
------------------------

<table>
	<tr>
		<td>Use Case</td>
		<td>“Cache This” button</td>
	</tr>
	<tr>
		<td>Actors</td>
		<td>USER, USER-AGENT</td>
	</tr>
	<tr>
		<td>Pre-condition</td>
		<td>Browser vendor builds a ‘cache this’ button. The process must save all associated files including scripts.</td>
	</tr>
	<tr>
		<td>Post-condition</td>
		<td>User can navigate and consume page whether offline or online and page has full capabilities whether online or offline (see note).</td>
	</tr>
	<tr>
		<td>Description</td>
		<td>A “Cache this” button could be added to the browser which will allow a user to cache the file associated with the URL and any assets associated with the URL.</td>
	</tr>
	<tr>
		<td>Need/Justification</td>
		<td>Currently if a user decided to “save” a page on Google Chrome then Chrome will automatically save the page (URL) and all the associated assets needed to make the page work. This could also work for web pages. Static information pages are easy – the information can be pulled down with the styling associated. This use case gets complicated when dealing with web apps which deal heavily with JavaScript (e.g. one page apps) as all JavaScript files will need to be taken and used as intended (although this shouldn’t be too different to pulling in CSS files).</td>
	</tr>
	<tr>
		<td>Note</td>
		<td>The big issue here is any XHR calls. These will not work. So, a method would need to be setup to deal with any XHR requests. Perhaps one solution would be in the case of forever-scrolling pages which use XHR to grab content; in this case if a page is offline the code could detect this and then say to the user ‘cannot load more content as you are offline’. 

		Need to check with browser vendors as to whether this is in any roadmap.
		</td>
	</tr>
</table>



<table>
	<tr>
		<td>Use Case</td>
		<td>Fallback</td>
	</tr>
	<tr>
		<td>Actors</td>
		<td>USER, DEVELOPER, WEB-APP, DEVICE, USER-AGENT</td>
	</tr>
	<tr>
		<td>Pre-condition</td>
		<td>
		</td>
	</tr>
	<tr>
		<td>Post-condition</td>
		<td></td>
	</tr>
	<tr>
		<td>Description</td>
		<td>When caching, a cache may be unable to locate an asset at the location in which it was cached. In these instances there should be a fallback. The fallbacks should come in two stages:
		1.	Fallback to the last saved cached file
		2.	Fallback to the version on the server</td>
	</tr>
	<tr>
		<td>Need/Justification</td>
		<td>There is a trade-off here to make; a user-agent could save every cached set of assets to fallback to, but this would mean a large amount of space would need to be dedicated to a set of saved caches. It was decided, therefore, that only one cache should be saved for a fallback, and all others should be deleted. </td>
	</tr>
	<tr>
		<td>Note</td>
		<td>Service Worker
In the Nav Controller it is up to the developer what they want to do with caching and fallbacks. Events have promises which give a response, so a developer could give a catch on an error which would initiate a network fetch which could give another promise, which could fallback to an old promise etc.

Note: Cache groups in Navigation Controller won't update till ALL CACHED ASSESTS have been pulled from the server and stored in the cache, so a new group replaces the old group, to make sure (e.g.) for a game, all assets are updated to your can play the level.
</td>
	</tr>
</table>

Online / Offline State Change Behaviour
---------------------------------------

<table>
	<tr>
		<td>Use Case</td>
		<td>Update Web App Assets</td>
	</tr>
	<tr>
		<td>Actors</td>
		<td>USER, DEVELOPER, WEB-APP, USER-AGENT</td>
	</tr>
	<tr>
		<td>Pre-condition</td>
		<td></td>
	</tr>
	<tr>
		<td>Post-condition</td>
		<td></td>
	</tr>
	<tr>
		<td>Description</td>
		<td>A DEVELOPER should be able to trigger the WEB-APP to “download” new HTML, CSS, JavaScript, images etc. when these are uploaded to the server</td>
	</tr>
	<tr>
		<td>Need/Justification</td>
		<td>One of the benefits of the web is the fact that the USER always sees the most up to date version of any web-app or web-site. When we start introducing complex caching methods the issue arises of when a web-app’s assets have been updated then how does a “cached” web app know this has happened? </td>
	</tr>
	<tr>
		<td>Note</td>
		<td>Not possible without polling or push. 

			Service Worker
			Can do this in navigation controller using fetch events and pulling things from the server if anything is available by comparing things in the server to things in the cache. Remember that controllers will only update when all tabs / windows using the controller have been closed. 
</td>
	</tr>
</table>


<table>
	<tr>
		<td>Use Case</td>
		<td>Moving from Offline to Online</td>
	</tr>
	<tr>
		<td>Actors</td>
		<td></td>
	</tr>
	<tr>
		<td>Pre-condition</td>
		<td>A method for detecting a whether a user has gone offline / online must be created and give correct results (no false positives or false negatives).</td>
	</tr>
	<tr>
		<td>Post-condition</td>
		<td>Events fire allowing developers to see correctly that a user is offline/online which means they can code solutions around this.</td>
	</tr>
	<tr>
		<td>Description</td>
		<td>When a USER moves from offline onto online, there must be a way for the WEB-APP to notify the server to tell the server what assets the USER consumed, and which assets it now needs.</td>
	</tr>
	<tr>
		<td>Need/Justification</td>
		<td>When a USER goes offline the WEB-APP can request assets be cached to allow the USER to access them whilst offline. However, when this USER moves back to an online state the WEB-APP should be able to notify the server to say “I am back online, here is what I did and what I now need”. For instance, if a USER is watching a US sitcom series of 24 episodes, they may download episodes “6-10” for their 2 hour plane flight. After the flight, the app will need to tell the server “update this USER’s history to say they watched episodes 6-10, and display this in their history, and remove them from the recommended list as they have been watched”. In certain cases the WEB-APP could also serve new “episodes” or other large assets, although it may be recommended that the DEVELOPER tests the USER’s bandwidth before downloading large assets. </td>
	</tr>
	<tr>
		<td>Note</td>
		<td></td>
	</tr>
</table>

Privileges and Priorities
-------------------------

<table>
	<tr>
		<td>Use Case</td>
		<td>User can give space privileges</td>
	</tr>
	<tr>
		<td>Actors</td>
		<td>USER, DEVELOPER, WEB-APP, DEVICE, USER-AGENT</td>
	</tr>
	<tr>
		<td>Pre-condition</td>
		<td>User agent and APIs must be setup to allow user to do give these permissions. These will need to be standardised. </td>
	</tr>
	<tr>
		<td>Post-condition</td>
		<td>User sees an alert which prompts them to allow or disallow an app to hold more space for it’s caches. </td>
	</tr>
	<tr>
		<td>Description</td>
		<td>A USER should be able to grant a WEB-APP that they trust with greater privileges related to space. Say (for example) a web-app has access to 10MB cache space but it really needs 20MB. The current caching solutions don’t allow for 20MB and for lots of reasons this is a good idea, as when a USER navigates to a new web-app they do not yet know how much they trust it. However, on using the web-app they can determine that they do trust the app and they want it to have access to extra storage to cache files for offline use or for performance gains (the objective is irrelevant). On the USER’s request the browser grants further access to storage to the web-app.</td>
	</tr>
	<tr>
		<td>Need/Justification</td>
		<td>Larger files and larger web-apps will require more space than the standard cache size to be able to function offline. </td>
	</tr>
	<tr>
		<td>Note</td>
		<td>Should the user or browser be able to revoke these privileges?

W3C Quota API
Being managed as part of the Quota API at W3C. 
</td>
	</tr>
</table>



Suggestions
-----------

If you wish to add any Use Cases please make a pull request.