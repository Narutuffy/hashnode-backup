## Sublime Text Editor Snippets for Bonfire Released


For those that use Bonfire and Sublime Text Editor, I have released my collection [**Snippet's**](https://github.com/svizion/bonfire-snippets) collection of just Bonfire snippets.  Since there is already a CodeIgniter Snippet package out there, I'm only including Bonfire specfic snippet's in the Package.  

Currently you have to install the packages manually with git

## Installation Instructions

~~~

	git clone git@github.com:svizion/bonfire-snippets.git ~/Library/Application\ Support/Sublime\ Text\ 2/Packages/bonfire-snippets

~~~

~~~

	git clone git@github.com:svizion/bonfire-snippets.git ~/.config/sublime-text-2/Packages/bonfire-snippets

~~~


## Current Snippets

These are the current snippets I've included, ordered in some fashion that sorta mad sense.

Update : I just included all my Model Snippets and my Debugging Snippets related to Bonfire.

### Template and Assets

<table class="table table-bordered table-condensed table-striped">
	<thead>
		<th>Tab Trigger</th><th>Output</th>
	</thead>
	<tbody>
	<tr>
		<td>set_data</td>
		<td>Template::set();</td>
	</tr>
	<tr>
		<td>render</td>
		<td>Template::render();</td>
	</tr>
	<tr>
		<td>toolbar_title</td>
		<td>Template::set('toolbar_title', '');</td>
	</tr>
	<tr>
		<td>clear_cache</td>
		<td>Assets::clear_cache();</td>
	</tr>
	<tr>
		<td>add_js</td>
		<td>Assets::add_js();</td>
	</tr>
	<tr>
		<td>add_css</td>
		<td>Assets::add_css();</td>
	</tr>
	<tr>
		<td>add_mod_js</td>
		<td>Assets::add_module_js();</td>
	</tr>
	<tr>
		<td>add_mod_css</td>
		<td>Assets::add_module_css();</td>
	</tr>
	</tbody>
</table>

### Various Randoms

<table class="table table-bordered table-condensed table-striped">
	<thead>
		<th>Tab Trigger</th><th>Output</th>
	</thead>
	<tbody>
	<tr>
		<td>cur_user</td>
		<td>$this->current_user->?;</td>
	</tr>		
	<tr>
		<td>bf_model</td>
		<td>Generates Bonfire Model Skeleton Structure</td>
	</tr>		
	<tr>
		<td>auth_controller</td>
		<td>Generates Authenticated_Controller Skeleton Structure</td>
	</tr>
	<tr>
		<td>haz_perm</td>
		<td>PHP If Wrapper for has_permission check</td></tr>
	</tr>		
	<tr>
		<td>restrict</td>
		<td>$this->auth->restrict();</td>
	</tr>
	<tr>
		<td>log_activity</td>
		<td>$this->activity_model->log_activity($this->current_user->user_id(), '', 'module');</td>
	</tr>
	<tr>
		<td>settings_set</td>
		<td>$this->settings_lib->set();</td>
	</tr>
	<tr>
		<td>settings_delete</td>
		<td>$this->settings_lib->delete();</td>
	</tr>
	</tbody>
</table>

### Bonfire Module Snippets

<table class="table table-bordered table-condensed table-striped">
	<thead>
		<th>Tab Trigger</th><th>Output</th>
	</thead>
	<tbody>
	<tr>
		<td>find</td>
		<td>$records = $this->$1_model->find($2);</td>
	</tr>
	<tr>
		<td>find_by</td>
		<td>$records = $this->$1_model->find_by('$1', '$2');</td>
	</tr>
	<tr>
		<td>find_all_by</td>
		<td>$records = $this->$1_model->find_all_by('$1', '$2');</td>
	</tr>
	<tr>
		<td>insert</td>
		<td>$id = $this->$1_model->insert($2);</td>
	</tr>
	<tr>
		<td>update</td>
		<td>$result = $this->$1_model->update($2, $data);</td>
	</tr>
	<tr>
		<td>update_where</td>
		<td>$result = $this->$1_model->update_where('$2', '$3', ${4:$data});</td>
	</tr>
	<tr>
		<td>delete</td>
		<td>$result = $this->$1_model->delete($2);</td>
	</tr>
	<tr>
		<td>delete_where</td>
		<td>$result = $this->$1_model->delete_where('$2', '$3');</td>
	</tr>

	<tr>
		<td>count_by</td>
		<td>$num = $this->$1_model->count_by('$2', '$3');</td>
	</tr>
	</tbody>
</table>

### Debugging snippets I use some are not fully Bonfire related.

<table class="table table-bordered table-condensed table-striped">
	<thead>
		<th>Tab Trigger</th><th>Output</th>
	</thead>
	<tbody>
	<tr>
		<td>cplog</td>
		<td>ChromePhp::log($1);</td>
	</tr>
	<tr>
		<td>cperror</td>
		<td>ChromePhp::error($1);</td>
	</tr>
	<tr>
		<td>console</td>
		<td>Console::log($1);</td>
	</tr>	
	<tr>
		<td>enable_profiler</td>
		<td>$this->output->enable_profiler(FALSE);</td>
	</tr>
	<tr>
		<td>bflog</td>
		<td>parent::logit($1, '${2:error]');</td>
	</tr>	
	<tr>
		<td>log</td>
		<td>logit($1, '${2:error]');</td>
	</tr>
	<tr>
		<td>dad</td>
		<td>$dump()die;</td>
	</tr>

	</tbody>
</table>


If you have any snippet's you use for Bonfire please fork and add them.  

