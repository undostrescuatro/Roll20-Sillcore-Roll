on("chat:message", function(msg) {
  //This allows players to enter !sr <number> to roll a number of d6 dices.
  if(msg.type == "api" && msg.content.indexOf("!sr ") !== -1) {
    var numdice = msg.content.replace("!sr ", "");
    var x="6";
    /*if(numdice.indexOf("x"!==-1){
        x=numdice.substring(numdice.indexOf("x")+1);
    } validate the existence of the paramenter x for exploding
     target end result is a string x=">x"
     this is usefull to count the number of sucesses
     working on it.
    */
    sendChat(msg.who, "/roll " + numdice + "d6sd>"+x, function(ops) {
    // ops will be an ARRAY of command results.
    var rollResult = ops[0]; // roll result is a json string of the roll
    var jRoll = JSON.parse(ops[0].content);
    var endResult = jRoll.rolls[0].results[0].v;
    if(jRoll.total>1){
        endResult+=jRoll.total-1;
    }
    sendChat(rollResult.who,"your result is: "+endResult.toString());
    });
  }
});