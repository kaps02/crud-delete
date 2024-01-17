<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>User Details Form</title>
    <style>
        .delete-icon {
            cursor: pointer;
            color: red;
            margin-left: 10px;
        }
    </style>
</head>
<body>
    <h1>Booking Appointment App</h1>
    <form id="user-detail" onsubmit="handleFormSubmit(event)">
        <label for="name">Name:</label>
        <input type="text" id="name" name="name">

        <label for="email">Email:</label>
        <input type="email" id="email" name="email">

        <label for="phone">Phone:</label>
        <input type="tel" id="phone" name="phone">
        <br>
        <button type="submit" id="sub">Submit</button>
    </form>

    <!-- Display user details below the form -->
    <ul id="userList"></ul>
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
    <script>
        function handleFormSubmit(event) {
            event.preventDefault();

            // Get user input values
            const username = document.getElementById('name').value;
            const email = document.getElementById('email').value;
            const phone = document.getElementById('phone').value;

            // Create user object
            const user = {
                username,
                email,
                phone
            };
            // Reset the input fields
            document.getElementById('name').value = '';
            document.getElementById('email').value = '';
            document.getElementById('phone').value = '';

            axios.post("https://crudcrud.com/api/7531730ffac34818a7ddd70e294179eb/appointment", user)
            .then((res) => {
                // Display users on the screen
                displayUsers(res.data);
                console.log(res);
            })
            .catch((err) => {
                console.log(err);
            });
        }

        window.addEventListener("DOMContentLoaded", () => {
            axios.get("https://crudcrud.com/api/7531730ffac34818a7ddd70e294179eb/appointment")
            .then((response) => {
                for (var i = 0; i < response.data.length; i++) {
                    displayUsers(response.data[i]);
                }
            })
            .catch((err) => {
                console.log(err);
            });
        });

        function displayUsers(user) {
            // Get the unordered list element
            const userList = document.getElementById('userList');

            // Create list item and append it to the unordered list
            const listItem = document.createElement('li');
            listItem.textContent = `${user.username} - ${user.email} - ${user.phone}`;

            // Create delete icon
            const deleteIcon = document.createElement('span');
            deleteIcon.innerHTML = '&#10006;'; // Unicode for the 'X' symbol
            deleteIcon.classList.add('delete-icon');
            deleteIcon.addEventListener('click', () => deleteUser(user._id, listItem));

            // Append delete icon to the list item
            listItem.appendChild(deleteIcon);

            // Set data attribute to identify the list item
            listItem.setAttribute('data-user-id', user._id);

            // Append list item to the unordered list
            userList.appendChild(listItem);
        }

        function deleteUser(userId, listItem) {
            axios.delete(`https://crudcrud.com/api/7531730ffac34818a7ddd70e294179eb/appointment/${userId}`)
            .then(() => {
                // Remove the deleted user from the UI
                if (listItem) {
                    listItem.remove();
                }
            })
            .catch((err) => {
                console.log(err);
            });
        }
    </script>
</body>
</html>


