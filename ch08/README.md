#LR5
##Chapter 8
###"Improving Forms"

This chapter builds on the "Guestbook" application by adding support for file uploads. In addition, form builders will be added to aid in the creation of custom forms.

To begin, open last app from Chapter 7 (guestbook06) in the text editor.

<sub>Alternatively, a new app can be created (i.e. "guestbook07") that is identical to _ch07/guestbook06_.</sub>

####"Adding a Picture by Uploading a File"
#####"File Upload Forms"

In the text editor, open the **people** form in app/views/people/_form.html.erb and add the following block of code:

		<div class="field">
			<%= f.label :photo %><br>
			<%= f.file_field :photo %>
		</div>

Since the form is now a "multipart" form, accepting attachment files along with post data, the form tag (at line 1 of the file) needs to reflect these changes. Modify the **form_for** method to read like this:

		<%= form_for(@person, :html => { :multipart => true }) do |f| %>

Remember to save the updated form before proceeding.

#####"Model and Migration Changes"
**A migration for an extension**

In the terminal, ensure that the prompt is at the current app (i.e. "guestbook") directory and run the following command:

		rails generate migration add_photo_extension_to_person

Locate the migration that was just created in the _db/migrate_ directory. Add the following line of code to the _change_ method:

		def change
			add_column :people, :extension, :string
		end

Run `rake db:migrate` to add the new column to the **:people** table.

**Strong parameters, again**

The migration has added the extension as a column in the database, however the information is still coming to the application as **:photo**. This means that **:photo** needs to be "whitelisted" in the private **person_params** method (at the end of *app/controllers/people_controller.rb*. The updated **person_params** method should look like this:

		# Never trust parameters from the scary internet, only allow the white list through.
		def person_params
			params.require(:person).permit(:name, :secret, :country,
				:email, :description, :can_send_email, 
				:graduation_year, :body_temperature, :price,
				:birthday, :favorite_time, :photo)
		end

**Extending a model beyond the database**



