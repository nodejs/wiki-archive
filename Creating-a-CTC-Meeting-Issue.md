# Setup

* npm install -g make-node-meeting
* brew install coreutils
* touch ~/.make-node-meeting/ctc.sh
* Get the contents from [the repo](https://github.com/rvagg/make-node-meeting/blob/master/examples/ctc.sh) (check for updates to members, etc.)

## Note

* As of this writing (July 2016), the current version of `make-node-meeting` has a bug such that you will need to run `<Path to global node_modules>/make-node-meeting/node_modules/.bin/node-meeting-agenda ctc-agenda` to generate a GitHub token once before running `make-node-meeting`. @rvagg will probably get around to fixing it, but not if you beat him to it and submit a pull request to https://github.com/rvagg/make-node-meeting.

# Every Week

* Go to the previous week's Google Doc
  * File -> Make A Copy
  * Check Share it with same people box
  * Fix the title of the doc
* Fix the date in the new doc
* Remove Present and Standup
* Move Agenda to Previous Meeting
* Delete Minutes and Q/A
* Update Upcoming Meetings (at least minimally)
* Update meeting time in `~/.make-node-meeting/ctc.sh`
* `make-node-meeting ctc`
* Copy and paste in new issue
* Label `ctc-meeting`
* Copy agenda into Google Doc
* Copy invited to present in Google Doc
* Set up StandUp in Google Doc
* Update issue number and link under Links at top of Google Doc

