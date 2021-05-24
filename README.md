# Le Wagon Prep: APIs

API = Application Programming Interface

APIs are designed to expose data in a machine-readable format (often JSON)

Webhooks are reversed API calls: the HTTP request is sent by the external service and received by your application
Example use case - Every 10 seconds, call the [Meetup](https://www.meetup.com/meetup_api/) API to fetch list of Meetup attendees.

- it would be expensive, in terms of server load and unncessary data, to keep calling the API every 10 seconds
- Alternate is the Hollywood Principle i.e. Don't call us, we'll call you
  - The Webhook would say "Meetup, please call my server as soon as a new member has RSVP'd"
  - Only new data is sent to your server, like a push message
  - Our app requires a urls destination where the webhook will push the data, `https://www.myapp.com/member_rsvpd`

## Resources

- [Programmable Web](https://www.programmableweb.com/category/all/apis) - list of APIs
- [Google Maps JS API](https://developers.google.com/maps/documentation/javascript/overview)
- [Snazzy Maps](https://snazzymaps.com/) - allows you to change Google Maps styles
- [Twilio](https://www.twilio.com/) - to send text/SMS messages
- [Google Geocoding API](https://developers.google.com/maps/documentation/geocoding/overview) - requires API key
- [Alternativce Geocoding APIs](https://github.com/alexreisner/geocoder/blob/master/README_API_GUIDE.md)
- [Guerilla Mail](https://www.guerrillamail.com/) - Disposable Temporary E-Mail Address
- [Wufoo](https://www.wufoo.com/) - form builder, requires paid plan
- [Typeform](https://www.typeform.com/) - form builder
- [Trello](https://trello.com/) - project management
- [Zapier](https://zapier.com/) - automate workflows
- [JSON Viewer](https://chrome.google.com/webstore/detail/json-viewer/gbmdgpbipfallnflgajpaliibnhdgobh) - chrome extension
- [RequestBin](https://requestbin.com/) - create a webhook to give toa service

## Examples

### Goecode

[Google Geocoding API](https://developers.google.com/maps/documentation/geocoding/overview) - requires API key

`http://maps.googleapis.com/maps/api/geocode/json?address=10%20Downing%20St,%20London`

[Nominatim](https://nominatim.org/release-docs/develop/api/Search/)

`https://nominatim.openstreetmap.org/ui/search.html?city=london&street=downing%20st`

### Twilio to send text messages

```ruby
# Sign up to twillio
# get API auth token

# This code would be run server-side

require 'twilio-ruby'

account_sid = 'AC______'
auth_token = '_________'

client = Twiio::REST::Client.new(account_sid, auth_token)

client.message.create(
    from: '+971528884444' # twilio provide a phone number
    to: '+971529993333'
    body: 'Hi buddy, you should attend Le Wagon'
)
```

### Webhooks example with Wufoo/ Typeform and Trello

Wufoo/ Typeform are form builder (drag and drop).
The form submission will be sent to Trello, a project management platform, where Sales staff picks the leads (form submission).

Create a fake email addres using Guerilla Mail to sign up on Typeform and Trello.

**Typform:**

- Create a Lead form
- Fields
  - name
  - email
  - What do you need?
- A URL to the live Form URL will be provided
- submissions will be available in the Typeform dashboard

**Trello:**

- Create a Trello account using Guerilla Mail email address
- Create a board e.g. a Leads Board
- Create lists for the board e.g. for leads the lists could be 'Inbox', 'Closing', 'Closed', 'Lost'
- You cna create some cards in the lists and drag them from one list to the other, as required

**Integrating Typeform with Trello:**

Typeform will play the role of Webhook, while Trello will play the role of API.

Zapier would be the middle man, if need be, to receive the data from Typeform and send it to Trello. Zapier would create a webhook in Typeform.

A Zapier account can be created using the Guerilla Mail email address.

In this case, Trello has a [native integration](https://www.typeform.com/connect/trello/) with Typeform, where you can map the Typeform fields to the lists in the Trello board.
When a Typeform submission occurs, a new card will be created in the list of the Trello board.

We can create a public endpoint on [RequestBin](https://requestbin.com/) to add in as a webhook in Typeform and see what an endpoint would typically receive from Typeform webhook. Then each time the form is submitted, this RequestBin endpoint will receive the form submission data.
