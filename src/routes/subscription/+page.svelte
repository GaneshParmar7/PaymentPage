<script>
	import {
		Button,
		DataTable,
		Dropdown,
		FileUploaderDropContainer,
		Form,
		FormGroup,
		Modal,
		RadioButton,
		RadioButtonGroup,
		Search,
		TextArea,
		TextInput
	} from 'carbon-components-svelte';
	import './sub.css';

	let subscription_start_date = '23 June 2023';
	let subscription_end_date = '23 June 2023';
	let plan_users = 75;
	let auto_renew = 'Yes';
	let subscription_plan = 'Enterprise';
	let subscribed_users = 9;
	let available_users = 66;

	let open = true;

	let email_disabled = false;
	let search_disabled = false;

	let search_txt = '';
	let email_txt = '';

    let selected="trusttalkuser";


	$: {
		if (search_txt != '') {
			email_disabled = true;
		} else {
			email_disabled = false;
		}
	}

	$: {
		if (email_txt != '') {
			search_disabled = true;
		} else {
			search_disabled = false;
		}
	}
</script>

<div class="main">
	<div class="main_content">
		<div class="flex">
			<div>
				<p>Subscription Start Date: <span>{subscription_start_date}</span></p>
				<p>Subscription End Date: <span>{subscription_end_date}</span></p>
				<p>Plan Users: <span>{plan_users}</span></p>
				<Button on:click={() => (open = true)} class="btn">Invite Users</Button>
			</div>
			<div>
				<p>Auto Renew: <span>{auto_renew}</span></p>
				<p>Subscription Plan: <span>{subscription_plan}</span></p>
				<p>Subscribed Users: <span>{subscribed_users}</span></p>
				<p>Available Users: <span>{available_users}</span></p>
			</div>
		</div>

		<div class="subscription_msg">
			<TextArea placeholder="Subscription message for sponsers" />

			<FileUploaderDropContainer
				class="file_upload"
				name="logo_file"
				labelText="Drag and drop your company logo or click to upload"
				validateFiles={(files) => {
					return files.filter((file) => file.size < 1_024);
				}}
				on:change={(e) => {
					console.log('files', e.detail);
				}}
			/>
		</div>

		<div class="subscription_table">
			<DataTable
				headers={[
					{ key: 'name', value: 'Name' },
					{ key: 'type', value: 'Type' },
					{ key: 'checked', value: 'Checked' },
					{ key: 'retract', value: 'Retract' }
				]}
				rows={[
					{
						id: 'a',
						name: 'Mohammed Lokhandwala',
						type: 'Social',
						checked: 'Yes',
						retract: 'Retract'
					},
					{
						id: 'b',
						name: 'Sashank Dewwdi',
						type: 'Social',
						checked: 'Yes',
						retract: 'Retract'
					},
					{
						id: 'c',
						name: 'Ganesh Parmar',
						type: 'Social',
						checked: 'No',
						retract: 'Retract'
					}
				]}
			/>
		</div>
	</div>
</div>

<Modal class="model" passiveModal bind:open modalHeading="Search Trusttalk user" on:open on:close>
	
    <Form>

        <RadioButtonGroup class="radio_group" orientation="vertical" legendText="Search by"  bind:selected>
            <RadioButton labelText="Trusttalk user name" value="trusttalkuser" />
            <RadioButton labelText="Email" value="email" />
            <RadioButton labelText="Linkedin Url" value="link" />
        </RadioButtonGroup>
    
        <!-- <Search disabled={search_disabled} bind:value={search_txt} placeholder="Search Trusttalk Users" /> -->
    
        <FormGroup class="email_invite">
            <TextInput
                bind:value={email_txt}
                disabled={email_disabled}
                class="email_input"
                type={selected=="email"?"email":"text"}
                name="user"
                required
            />
            <Button type="submit" class="invite_btn">Send Invitation</Button>
        </FormGroup>
    </Form>
</Modal>

<style>
	.main {
		display: flex;
		justify-content: center;
		align-items: center;
		height: 100vh;
	}
	.main_content {
		max-width: 900px;
		width: 100%;
		margin: 0 auto;
		padding: 1em;
	}
	.flex {
		display: flex;
		justify-content: space-evenly;
	}
	p {
		margin: 1em 0;
	}
	.subscription_table {
		margin-top: 2.5em;
	}

	@media (max-width: 768px) {
		.flex {
			display: block;
			max-width: 300px;
			width: 100%;
			margin: 0 auto;
		}
	}
</style>
