<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>EV Customer List</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/css/bootstrap.min.css" />
  <script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
  <style>
    body {
      padding: 20px;
    }
    .table td, .table th {
      vertical-align: middle;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2 class="mb-4">EV Customer List</h2>
    <button class="btn btn-primary mb-3" data-toggle="modal" data-target="#customerModal">Add Customer</button>
    <table class="table table-bordered">
      <thead class="thead-dark">
        <tr>
          <th>Name</th>
          <th>Email</th>
          <th>Phone</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody id="customerTable"></tbody>
    </table>
  </div>

  <!-- Modal -->
  <div class="modal fade" id="customerModal" tabindex="-1" role="dialog">
    <div class="modal-dialog" role="document">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title">Customer Form</h5>
          <button type="button" class="close" data-dismiss="modal">&times;</button>
        </div>
        <div class="modal-body">
          <form id="customerForm">
            <input type="hidden" id="customerIndex">
            <div class="form-group">
              <label>Name</label>
              <input type="text" class="form-control" id="name" required>
            </div>
            <div class="form-group">
              <label>Email</label>
              <input type="email" class="form-control" id="email" required>
            </div>
            <div class="form-group">
              <label>Phone</label>
              <input type="text" class="form-control" id="phone" required>
            </div>
            <button type="submit" class="btn btn-success">Save</button>
          </form>
        </div>
      </div>
    </div>
  </div>

  <script>
    let customers = [];

    function renderTable() {
      const table = $('#customerTable');
      table.empty();
      customers.forEach((customer, index) => {
        table.append(`<tr>
          <td>${customer.name}</td>
          <td>${customer.email}</td>
          <td>${customer.phone}</td>
          <td>
            <button class="btn btn-sm btn-info" onclick="editCustomer(${index})">Edit</button>
            <button class="btn btn-sm btn-danger" onclick="deleteCustomer(${index})">Delete</button>
          </td>
        </tr>`);
      });
    }

    $('#customerForm').on('submit', function(e) {
      e.preventDefault();
      const name = $('#name').val();
      const email = $('#email').val();
      const phone = $('#phone').val();
      const index = $('#customerIndex').val();

      if (index === '') {
        customers.push({ name, email, phone });
      } else {
        customers[index] = { name, email, phone };
      }

      $('#customerModal').modal('hide');
      this.reset();
      $('#customerIndex').val('');
      renderTable();
    });

    function editCustomer(index) {
      const customer = customers[index];
      $('#name').val(customer.name);
      $('#email').val(customer.email);
      $('#phone').val(customer.phone);
      $('#customerIndex').val(index);
      $('#customerModal').modal('show');
    }

    function deleteCustomer(index) {
      if (confirm('Are you sure you want to delete this customer?')) {
        customers.splice(index, 1);
        renderTable();
      }
    }
  </script>

  <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
