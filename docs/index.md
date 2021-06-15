This page will soon display the test suite performance of the language models evaluated in 

Anne Beyer, Sharid Loáiciga and David Schlangen (2021). **Is Incoherence Surprising? Targeted Evaluation of Coherence Prediction from Language Models.** To appear in: _Proceedings of the 2021 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies, Volume 1 (Long and Short Papers)_, June 6-11, Online.

### What is a test suite?


### Which phenomena are covered?


### Which language models can be evaluated?


### Results on test suites

<script type="text/javascript" src="jquery.js"></script>

<script type="text/javascript">

var tag_data;

$( document ).ready(function() {

  $( document ).load("visualization/tags.json", function(responseTxt, statusTxt, xhr){
    console.log(responseTxt);
    tag_data = JSON.parse(responseTxt);
    let suites = Object.keys(tag_data);
    let suite_select = document.getElementById("suite-select");
    for(let i = 0; i < suites.length; i++)
    {
      let new_opt = document.createElement("option");
      new_opt.text = suites[i];
      suite_select.add(new_opt);
    } 
    
    suite_select.value = suites[0];
    curr_suite = suites[0];

    let metrics = Object.keys(tag_data[curr_suite]["tags"]);
    let metric_select = document.getElementById("metric-select");
    for(let i = 0; i < metrics.length; i++)
    {
      let new_opt = document.createElement("option");
      new_opt.text = metrics[i];
      metric_select.add(new_opt);
    }

    metric_select.value = metrics[0];
    curr_metric = metrics[0];

    let subsets = Object.keys(tag_data[curr_suite]["tags"][curr_metric]);
    let subset_select = document.getElementById("subset-select");
    for(let i = 0; i < subsets.length; i++)
    {
      let new_opt = document.createElement("option");
      new_opt.text = subsets[i];
      subset_select.add(new_opt);
    }

    subset_select.value = subsets[0];
    curr_subset = subsets[0];

    let graphics_div = document.getElementById("graphic");
    let script = document.createElement("script");
    let curr_tag = tag_data[curr_suite]["tags"][curr_metric][curr_subset];
    script.src = "visualization/" + curr_tag["src"];
    script.id = curr_tag["id"];
    script.name = "bokeh-script";
    graphics_div.appendChild(script);


    //update example
    let example = tag_data[curr_suite]["example"]["conditions"];
    let orig = example[0];
    let orig_name = orig["condition_name"];
    let orig_content = orig["regions"][0]["content"] + " " + orig["regions"][1]["content"];
    let orig_div = document.getElementById("orig");
    orig_div.innerHTML = orig_content;
    
    let pert = example[1];
    let pert_name = pert["condition_name"];
    let pert_content = pert["regions"][0]["content"] + " " + pert["regions"][1]["content"];
    let pert_div = document.getElementById("pert");
    pert_div.innerHTML = pert_content;
  });
  
})

// change of suite
$( document ).ready(function() {
    $(document).on('change', '#suite-select', function(){
        event.preventDefault();
        
        //let suite_select = document.getElementById("suite-select");
        curr_suite = this.value;

        let metrics = Object.keys(tag_data[curr_suite]["tags"]);
        let metric_select = document.getElementById("metric-select");
        while (metric_select.options.length > 0)
        {
            metric_select.remove(0);
        }
        for(let i = 0; i < metrics.length; i++)
        {
          let new_opt = document.createElement("option");
          new_opt.text = metrics[i];
          metric_select.add(new_opt);
        }

        metric_select.value = metrics[0];
        curr_metric = metrics[0];


        let subset_select = document.getElementById("subset-select");
        curr_subset = subset_select.value;

        let graphics_div = document.getElementById("graphic");
        //graphics_div.removeChild(graphics_div.childNodes[0]);
        //let script = document.getElementsByName("bokeh-script")[0];
        //console.log(script);
	//graphics_div.removeChild(script);
        //script.remove();
        while (graphics_div.lastChild) {
          graphics_div.removeChild(graphics_div.lastChild);
        }
	let script_new = document.createElement("script");
        console.log(curr_suite, curr_metric, curr_subset);
        let curr_tag = tag_data[curr_suite]["tags"][curr_metric][curr_subset];
        script_new.src = "visualization/" + curr_tag["src"];
        script_new.id = curr_tag["id"];
        script_new.name = "bokeh-script";
        graphics_div.appendChild(script_new);

        //update example
        let example = tag_data[curr_suite]["example"]["conditions"];
        let orig = example[0];
        let orig_name = orig["condition_name"];
        let orig_content = orig["regions"][0]["content"] + " " + orig["regions"][1]["content"];
        let orig_div = document.getElementById("orig");
        orig_div.innerHTML = orig_content;

        let pert = example[1];
        let pert_name = pert["condition_name"];
        let pert_content = pert["regions"][0]["content"] + " " + pert["regions"][1]["content"];
        let pert_div = document.getElementById("pert");
        pert_div.innerHTML = pert_content;
    })
});


// change of metric or subset
$( document ).ready(function() {
    $(document).on('change', '#select-form', function(){
        event.preventDefault();
        
        let suite_select = document.getElementById("suite-select");
        curr_suite = suite_select.value;
        let metric_select = document.getElementById("metric-select");
        curr_metric = metric_select.value;
        let subset_select = document.getElementById("subset-select");
        curr_subset = subset_select.value;

        let graphics_div = document.getElementById("graphic");
        //graphics_div.removeChild(graphics_div.childNodes[0]);
        //let script = document.getElementsByName("bokeh-script")[0];
        //console.log(script);
	//graphics_div.removeChild(script);
        //script.remove();
        while (graphics_div.lastChild) {
          graphics_div.removeChild(graphics_div.lastChild);
        }
	let script_new = document.createElement("script");
        console.log(curr_suite, curr_metric, curr_subset);
        let curr_tag = tag_data[curr_suite]["tags"][curr_metric][curr_subset];
        script_new.src = "visualization/" + curr_tag["src"];
        script_new.id = curr_tag["id"];
        script_new.name = "bokeh-script";
        graphics_div.appendChild(script_new);
        
    })
});


</script>

You can select a suite, a metric, by which the models where evaluated, and a subset of examples: "All", "True" (i.e. pairs the model got correct), and "False" (pairs the model did not get correctly).

<form id="select-form">
Suites:
<select id="suite-select">

</select>
<br>
Metrics:
<select id="metric-select">

</select>
<br>
Subset:
<select id="subset-select">

</select>

</form>


<div id="graphic">

</div>

#### An example from the currently selected test suite:
**Unperturbed dialogue:**

<div id="orig">

</div>

**Perturbed dialogue:**

<div id="pert">

</div>
