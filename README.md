<html>
<font color="#0000FF" size="15">
<font color="#0000FF" size="15">Countdown Timer: </font> &nbsp&nbsp&nbsp; <span id="showmns"><font color="#0000FF" size="17">00
</font></span>:<span id="showscs"><font color="#0000FF" size="17">00</font></span> &nbsp&nbsp&nbsp
<font color="#0000FF" size="15">Minutes: </font><font color="#0000FF" size="17"> <input type="text" id="mns" name="mns" value="1440" size="3" maxlength="3" /> </font>&nbsp; &nbsp; <font color="#0000FF" size="17"> Seconds: </font> <input type="text" id="scs" name="scs" value="0" size="2" maxlength="2" />
<form action="#" method="post">
<input type="button" id="btnct" value="START" />
<input type="button" id="btnct_res" value="RESTART" />
<input type="button" id="btnct_end" value="END" />
</form>
<script>
// <![CDATA[
function CountdownTimer(obnm){
// http://coursesweb.net/javascript/
  var endct =0;  // it is set to 1 when script starts
  var ctmnts =0;  // minutes
  var ctsecs =0;  // seconds
  var startchr =0;  // used to control when to read data from form, and script finished
  var ctpause =-1;  //if -1, pause the script

  //get html elms.
  var el_showmns = document.getElementById('showmns');
  var el_showscs = document.getElementById('showscs');
  var el_mns = document.getElementById('mns');
  var el_scs = document.getElementById('scs');
  var el_btnct = document.getElementById('btnct');
  var el_btnct_res = document.getElementById('btnct_res');
  var el_btnct_end = document.getElementById('btnct_end');

  //to start/pause/resume Countdown Timer
  function startPauseCT(){
    if(parseInt(el_mns.value) >0 || parseInt(el_scs.value)>0 || endct ==1){
      ctpause *=-1;
      if(ctpause ==1){ //Start and set next click as Pause
        el_btnct.value ='PAUSE';
        window[obnm].countdownTimer();
      }
      else el_btnct.value ='RESUME';
    }
  }

  // HERE YOU CAN ADD TO EXECUTE JavaScript instructions WHEN COUNTDOWN TIMER REACHES TO 0
  function endCT(){
    // HERE ADD YOUR CODE

    return false;
  }

  this.countdownTimer = function(){
    // if $startchr is 0, and form fields exists, gets data for minutes and seconds, and sets $startchr to 1
    if(startchr == 0 && el_mns && el_scs) {
      // makes sure the script uses integer numbers
      ctmnts = parseInt(el_mns.value);
      ctsecs = parseInt(el_scs.value);

      // if data not a number, sets the value to 0
      if(isNaN(ctmnts)) ctmnts = 0;
      if(isNaN(ctsecs)) ctsecs = 0;

      // rewrite data in form fields to be sure that the fields for minutes and seconds contain integer number
      el_mns.value = ctmnts;
      el_scs.value = ctsecs;
      startchr = 1;
    }

    if(ctmnts >0 || ctsecs >0) endct =1;  //to can call endCT() at the ending

    // if minutes and seconds are 0, call endCT()
    if(ctmnts==0 && ctsecs==0 && endct ==1){
      startchr =0;
      ctpause =-1;
      endct =0;
      el_btnct.value ='START';
      endCT();
    }
    else if(startchr ==1 && ctpause ==1){
      // decrease seconds, and decrease minutes if seconds reach to 0
      ctsecs--;
      if(ctsecs < 0){
        if(ctmnts > 0) {
          ctsecs = 59;
          ctmnts--;
        }
        else {
          ctsecs = 0;
          ctmnts = 0;
        }
      }
      setTimeout(obnm +'.countdownTimer()', 1000); //auto-calls this function after 1 seccond
    }

    // display the time in page
    el_showmns.innerHTML = ctmnts;
    el_showscs.innerHTML = ctsecs;
  }

  //set event to button that starts the Countdown Timer
  if(el_btnct) el_btnct.addEventListener('click', startPauseCT);

  //restart Countdown Timer from the initial values
  if(el_btnct_res) el_btnct_res.addEventListener('click', function(){ startchr =0; if(ctpause ==-1) startPauseCT(); });

  //ending Countdown Timer, sets 0 form data
  if(el_btnct_end) el_btnct_end.addEventListener('click', function(){ el_mns.value =0; el_scs.value =0; startchr =0; startPauseCT(); });
}

//set object of CountdownTimer class
var obCT = new CountdownTimer('obCT');
// ]]>
</script>
</font>
</html>
