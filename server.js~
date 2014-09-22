var express = require('express');
var app = express();


var player_list = {};
var player_inv =  {};
var player_loc = {};
var players_in_loc = {};

app.get('/', function(req, res){
	res.status(200);
	res.sendFile(__dirname + "/index.html");
});



app.get('/:userid/:id', function(req, res){
	if (req.params.userid == "")
		return;
	var resp = "";
	var currloc = "strong-hall";
	console.log(player_loc[req.params.userid]);
	console.log(player_inv[req.params.userid]);
	if (req.params.id == "inventory") {
	    res.set({'Content-Type': 'application/json'});
	    res.status(200);
	    res.send(player_inv[req.params.userid]);
	    return;
	}
	if (req.params.id == "people") {
		currloc = player_loc[req.params.userid];
		for (var j in players_in_loc) {
			players_in_loc[j] = [];
		}
		for (var k in player_loc) {
			if (player_loc[k] == currloc) {
				if (players_in_loc[currloc] == undefined) {
					players_in_loc[currloc] = [];
					players_in_loc[currloc].push(k);
				}
				else {
					players_in_loc[currloc].push(k)
				}
			}
		}
		resp = "";
		for (var j in players_in_loc) {
			resp += players_in_loc[j];
		}
	    res.set({'Content-Type': 'application/json'});
	    res.status(200);
	    res.send(resp);
	    return;
	}
	for (var i in campus) {
		if (req.params.id == campus[i].id) {
		    res.set({'Content-Type': 'application/json'});
		    res.status(200);
		    player_loc[req.params.userid] = campus[i].id;
		    res.send(campus[i]);
		    return;
		}
	}
	res.status(404);
	res.send("not found, sorry");
});

app.get('/:userid', function(req, res) {
	if (player_list[req.params.userid] === undefined) {
		player_list[req.params.userid] = req.params.userid;
		userid = req.params.userid;
		player_inv[req.params.userid] = ["laptop"];
		player_loc[req.params.userid] = campus[4].id;
	}

});

app.get('/:userid/images/:name', function(req, res){
	res.status(200);
	res.sendFile(__dirname + "/" + req.params.name);
});

app.delete('/:userid/:id/:item', function(req, res){
	for (var i in campus) {
		if (req.params.id == campus[i].id) {
		    res.set({'Content-Type': 'application/json'});
		    var ix = -1;
		    if (campus[i].what != undefined) {
					ix = campus[i].what.indexOf(req.params.item);
		    }
		    if (ix >= 0) {
		       res.status(200);
			player_inv[req.params.userid].push(campus[i].what[ix]); // stash
		        res.send(player_inv[req.params.userid]);
			campus[i].what.splice(ix, 1); // room no longer has this
			return;
		    }
		    res.status(200);
		    res.send([]);
		    return;
		}
	}
	res.status(404);
	res.send("location not found");
});

app.put('/:userid/:id/:item', function(req, res){
	for (var i in campus) {
		if (req.params.id == campus[i].id) {
				// Check you have this
				var ix = player_inv[req.params.userid].indexOf(req.params.item)
				if (ix >= 0) {
					dropbox(req.params.userid,ix,campus[i]);
					res.set({'Content-Type': 'application/json'});
					res.status(200);
					res.send([]);
				} else {
					res.status(404);
					res.send("you do not have this");
				}
				return;
		}
	}
	res.status(404);
	res.send("location not found");
});

app.listen(3000);

var dropbox = function(id, ix,room) {
	var item = player_inv[id][ix];
	player_inv[id].splice(ix, 1);	 // remove from inventory
	if (room.id == 'allen-fieldhouse' && item == "basketball") {
		room.text	+= " Someone found the ball so there is a game going on!"
		return;
	}
	if (room.what == undefined) {
		room.what = [];
	}
	room.what.push(item);
}

var campus =
    [ { "id": "lied-center",
	"where": "LiedCenter.jpg",
	"next": {"east": "eaton-hall", "south": "dole-institute"},
	"text": "You are outside the Lied Center."
      },
      { "id": "dole-institute",
	"where": "DoleInstituteofPolitics.jpg",
	"next": {"east": "allen-fieldhouse", "north": "lied-center"},
	"text": "You take in the view of the Dole Institute of Politics. This is the best part of your walk to Nichols Hall."
      },
      { "id": "eaton-hall",
	"where": "EatonHall.jpg",
	"next": {"east": "snow-hall", "south": "allen-fieldhouse", "west": "lied-center"},
	"text": "You are outside Eaton Hall. You should recognize here."
      },
      { "id": "snow-hall",
	"where": "SnowHall.jpg",
	"next": {"east": "strong-hall", "south": "ambler-recreation", "west": "eaton-hall"},
	"text": "You are outside Snow Hall. Math class? Waiting for the bus?"
      },
      { "id": "strong-hall",
	"where": "StrongHall.jpg",
	"next": {"east": "outside-fraser", "north": "memorial-stadium", "west": "snow-hall"},
	"what": ["coffee"],
	"text": "You are outside Stong Hall."
      },
      { "id": "ambler-recreation",
	"where": "AmblerRecreation.jpg",
	"next": {"west": "allen-fieldhouse", "north": "snow-hall"},
	"text": "It's the starting of the semester, and you feel motivated to be at the Gym. Let's see about that in 3 weeks."
      },
      { "id": "outside-fraser",
  "where": "OutsideFraserHall.jpg",
	"next": {"west": "strong-hall","north":"spencer-museum"},
	"what": ["basketball"],
	"text": "On your walk to the Kansas Union, you wish you had class outside."
      },
      { "id": "spencer-museum",
	"where": "SpencerMuseum.jpg",
	"next": {"south": "outside-fraser","west":"memorial-stadium"},
	"what": ["art"],
	"text": "You are at the Spencer Museum of Art."
      },
      { "id": "memorial-stadium",
	"where": "MemorialStadium.jpg",
	"next": {"south": "strong-hall","east":"spencer-museum"},
	"what": ["ku flag"],
	"text": "Half the crowd is wearing KU Basketball gear at the football game."
      },
      { "id": "allen-fieldhouse",
	"where": "AllenFieldhouse.jpg",
	"next": {"north": "eaton-hall","east": "ambler-recreation","west": "dole-institute"},
	"text": "Rock Chalk! You're at the field house."
      }
    ]

