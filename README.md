# Whisk Information Timeline Tool (WITT)

## Goal
The goal of Witt is to explore new visualizations and analysis of OpenWhisk's activation logs to support debugging and program understanding. 

## Demo Video
Scenario: Viewing 200 activations of a chatbot application that was implemented as a sequence. Using the timeline view to understand its composition & find the most time-consuming action. Using the summary view to view all activations of that action, how its output data changed with respect to different output actions, and get the slowest activation of the action. Switching back to the timeline view to locate that activation and view the entire sequence. 

[https://youtu.be/P3iROXjMKN8](https://youtu.be/P3iROXjMKN8)

## Install
```
npm install witt -g 
```

## Run
By default, Witt retrieves the most recent 20 activations. Simply run
```
$ witt
```

Use `limit` to specify the number of recent activations retrieved. Max is 2000 (it might take a while if the number is more than 200) 
```
$ witt --limit=100
```

Use `file` to read activations from a local file. I'll explain this more later. 
```
$ witt --file=data.json
```

Use `key` to provide a OpenWhisk key manually
```
$ witt --key=XXXXXXXXXXXXX
```

After getting activations from the OpenWhisk server or the assigned local file, the system will open a web page (Witt web UI) in your browser. If the activations come from the OpenWhisk server, you can refresh the web page to retrieve new activations. To quit Witt, close the web page in your browser, and Ctrl-C to end Witt in the terminal. 

## Witt Web UI
The Web UI provides 3 views of the activations data: timeline view, list view, and summary view. You can select the view using the buttons at the top. Currently selected view is in green, other buttons in grey.

1. Timeline View:
![Timeline view screenshot](https://media.github.ibm.com/user/26582/files/5c14f290-4542-11e7-91a2-050979aedbe5)

	The timeline view has a histogram and a timeline visualization. The histogram shows the distribution of the retrieved activations over time. In the histogram, green bars are successful activations, red bars are failed activations. The bar that contains the activation displayed in the right pane is in yellow. The user can drag a rectangle in the histogram to narrow down the number of activations shown in the timeline below. 

	In the timeline visualization, each activation is a bar color-coded to show if its a trigger, rule, sequence or action activation. The length of the bar represents it activation's duration. A sequence bar has a dashed rectangle below it to group all the child actions/sequences it invoked. Hovering/clicking on a timeline bar will show the detail information of the activation in the right pane. In the right pane, the user can choose to view a rendered version of the activation or the raw JSON activation file.

2. List View:
![List view screenshot](https://media.github.ibm.com/user/26582/files/6610503a-4544-11e7-8519-7a3e3d1d7cb5)

	The list view shows the activations in a familiar list UI. Most recent activations are on the top of the list. Interacting with the list view is similar to interacting with the timeline view. 

3. Summary View (experimental):
![Summary view screenshot](https://media.github.ibm.com/user/26582/files/84269a5c-4544-11e7-8da0-d4bbda9edc5a)

	The summary view lets the user select an action and view some summary data generated by aggregating the retrieved activations. The summary view shows average, median, min and max execution time of an action; all the actions that ran before the selected action; all the actions that ran after the selected action; and a summary of the action's output data. Watch the demo video for how this summary view might be useful to you. 

### Exporting the selected data
The user can click on the "Export Selected Data" button (a blue button) at the top to export activation logs *shown in the histogram* to a local JSON file. The file can then be loaded back to Witt using the `file` attribute from the terminal for viewing. This feature can be useful if you want to share the data with other people to debug or explain your Whisk actions. 

Note that the exported file will only include the activations that are shown in the histogram. This is useful if you want to exclude certain activations (and generate a cleaner graph) for sharing. If you want to include all the retrieved activations, remember to click on "Reset selection" to reset the histogram before exporting the file. Also, the exported data are not raw OpenWhisk activation logs. They are data processed by Witt. 

## Limitations
Currently, Witt cannot establish connections among activations that were dynamically invoked from the code. This is because OpenWhisk does not record the caller/callee information for dynamically-invoked activations in the logs. I'm working on ways to use the developer's log to track the caller/callee and visualize it in Witt.



	





