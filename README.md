<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Restaurant Scheduling Assistant</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        :root {
            --primary: #4361ee;
            --secondary: #3f37c9;
            --success: #4cc9f0;
            --danger: #f72585;
            --warning: #f8961e;
            --info: #4895ef;
            --light: #f8f9fa;
            --dark: #212529;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f5f7fa;
            color: #333;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        
        .card {
            border-radius: 12px;
            box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
            margin-bottom: 24px;
            border: none;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        
        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 12px 20px rgba(0, 0, 0, 0.15);
        }
        
        .btn {
            border-radius: 8px;
            padding: 10px 20px;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            transition: all 0.3s ease;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        
        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 8px rgba(0, 0, 0, 0.15);
        }
        
        .btn-primary {
            background-color: var(--primary);
            border-color: var(--primary);
        }
        
        .btn-success {
            background-color: var(--success);
            border-color: var(--success);
        }
        
        .schedule-grid {
            display: grid;
            grid-template-columns: 150px repeat(7, 1fr);
            gap: 10px;
            margin-top: 20px;
        }
        
        .schedule-header {
            font-weight: bold;
            text-align: center;
            padding: 10px;
            background-color: var(--primary);
            color: white;
            border-radius: 8px;
        }
        
        .employee-row {
            display: contents;
        }
        
        .employee-name {
            display: flex;
            align-items: center;
            padding: 10px;
            background-color: var(--light);
            border-radius: 8px;
            font-weight: 600;
        }
        
        .schedule-cell {
            padding: 10px;
            background-color: white;
            border-radius: 8px;
            min-height: 60px;
            cursor: pointer;
            transition: background-color 0.2s ease;
        }
        
        .schedule-cell:hover {
            background-color: #e9ecef;
        }
        
        .overtime-indicator {
            width: 8px;
            height: 24px;
            border-radius: 4px;
            margin-right: 10px;
            background-color: #4cc9f0;
        }
        
        .overtime-warning {
            background-color: #f8961e;
        }
        
        .overtime-danger {
            background-color: #f72585;
        }
        
        .modal-content {
            border-radius: 12px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
        }
        
        .dropzone {
            border: 2px dashed #ccc;
            border-radius: 8px;
            padding: 30px;
            text-align: center;
            cursor: pointer;
            transition: border-color 0.3s ease;
        }
        
        .dropzone:hover {
            border-color: var(--primary);
        }
    </style>
</head>
<body>
    <div class="container mt-4">
        <header class="mb-5 text-center">
            <h1 class="display-4">Restaurant Scheduling Assistant</h1>
            <p class="lead">Automate your staff scheduling with AI-powered optimization</p>
        </header>
        
        <div class="row">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header bg-white d-flex justify-content-between align-items-center">
                        <h5 class="mb-0">Weekly Schedule</h5>
                        <div>
                            <button class="btn btn-primary me-2" id="generate-schedule">Generate Schedule</button>
                            <div class="dropdown d-inline-block">
                                <button class="btn btn-success dropdown-toggle" type="button" id="shareDropdown" data-bs-toggle="dropdown" aria-expanded="false">
                                    Share Schedule
                                </button>
                                <ul class="dropdown-menu" aria-labelledby="shareDropdown">
                                    <li><a class="dropdown-item" href="#" id="share-email">Email</a></li>
                                    <li><a class="dropdown-item" href="#" id="share-text">Text Message</a></li>
                                    <li><a class="dropdown-item" href="#" id="share-whatsapp">WhatsApp Group</a></li>
                                </ul>
                            </div>
                        </div>
                    </div>
                    <div class="card-body">
                        <div class="schedule-grid">
                            <div class="schedule-header">Employee</div>
                            <div class="schedule-header">Monday</div>
                            <div class="schedule-header">Tuesday</div>
                            <div class="schedule-header">Wednesday</div>
                            <div class="schedule-header">Thursday</div>
                            <div class="schedule-header">Friday</div>
                            <div class="schedule-header">Saturday</div>
                            <div class="schedule-header">Sunday</div>
                            
                            <!-- Sample employee rows -->
                            <div class="employee-row">
                                <div class="employee-name">
                                    <div class="overtime-indicator"></div>
                                    John Smith
                                </div>
                                <div class="schedule-cell" data-employee="john" data-day="monday">Off</div>
                                <div class="schedule-cell" data-employee="john" data-day="tuesday">10:00 - 18:00</div>
                                <div class="schedule-cell" data-employee="john" data-day="wednesday">10:00 - 18:00</div>
                                <div class="schedule-cell" data-employee="john" data-day="thursday">10:00 - 18:00</div>
                                <div class="schedule-cell" data-employee="john" data-day="friday">10:00 - 18:00</div>
                                <div class="schedule-cell" data-employee="john" data-day="saturday">12:00 - 20:00</div>
                                <div class="schedule-cell" data-employee="john" data-day="sunday">Off</div>
                            </div>
                            
                            <div class="employee-row">
                                <div class="employee-name">
                                    <div class="overtime-indicator overtime-warning"></div>
                                    Sarah Johnson
                                </div>
                                <div class="schedule-cell" data-employee="sarah" data-day="monday">12:00 - 20:00</div>
                                <div class="schedule-cell" data-employee="sarah" data-day="tuesday">12:00 - 20:00</div>
                                <div class="schedule-cell" data-employee="sarah" data-day="wednesday">Off</div>
                                <div class="schedule-cell" data-employee="sarah" data-day="thursday">12:00 - 20:00</div>
                                <div class="schedule-cell" data-employee="sarah" data-day="friday">12:00 - 20:00</div>
                                <div class="schedule-cell" data-employee="sarah" data-day="saturday">16:00 - 00:00</div>
                                <div class="schedule-cell" data-employee="sarah" data-day="sunday">16:00 - 00:00</div>
                            </div>
                            
                            <div class="employee-row">
                                <div class="employee-name">
                                    <div class="overtime-indicator overtime-danger"></div>
                                    Miguel Rodriguez
                                </div>
                                <div class="schedule-cell" data-employee="miguel" data-day="monday">16:00 - 00:00</div>
                                <div class="schedule-cell" data-employee="miguel" data-day="tuesday">16:00 - 00:00</div>
                                <div class="schedule-cell" data-employee="miguel" data-day="wednesday">16:00 - 00:00</div>
                                <div class="schedule-cell" data-employee="miguel" data-day="thursday">Off</div>
                                <div class="schedule-cell" data-employee="miguel" data-day="friday">16:00 - 00:00</div>
                                <div class="schedule-cell" data-employee="miguel" data-day="saturday">16:00 - 00:00</div>
                                <div class="schedule-cell" data-employee="miguel" data-day="sunday">Off</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="col-md-4">
                <div class="card mb-4">
                    <div class="card-header bg-white">
                        <h5 class="mb-0">Add Employee</h5>
                    </div>
                    <div class="card-body">
                        <form id="employee-form">
                            <div class="mb-3">
                                <label for="employee-name" class="form-label">Name</label>
                                <input type="text" class="form-control" id="employee-name" required>
                            </div>
                            <div class="mb-3">
                                <label for="employee-position" class="form-label">Position</label>
                                <select class="form-select" id="employee-position" required>
                                    <option value="">Select position</option>
                                    <option value="server">Server</option>
                                    <option value="bartender">Bartender</option>
                                    <option value="host">Host/Hostess</option>
                                    <option value="cook">Cook</option>
                                    <option value="dishwasher">Dishwasher</option>
                                    <option value="manager">Manager</option>
                                </select>
                            </div>
                            <div class="mb-3">
                                <label for="employee-contact" class="form-label">Contact (Email/Phone)</label>
                                <input type="text" class="form-control" id="employee-contact" required>
                            </div>
                            <button type="submit" class="btn btn-primary w-100">Add Employee</button>
                        </form>
                    </div>
                </div>
                
                <div class="card">
                    <div class="card-header bg-white">
                        <h5 class="mb-0">Upload Special Events</h5>
                    </div>
                    <div class="card-body">
                        <div class="dropzone" id="pdf-dropzone">
                            <i class="fas fa-file-pdf fa-3x mb-3"></i>
                            <p>Drag & drop PDFs here or click to upload</p>
                            <p class="text-muted small">Upload DEO files for private parties and special events</p>
                            <input type="file" id="pdf-upload" accept=".pdf" class="d-none">
                        </div>
                        <div class="mt-3" id="uploaded-files">
                            <!-- Uploaded files will appear here -->
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    
    <!-- Schedule Edit Modal -->
    <div class="modal fade" id="scheduleModal" tabindex="-1" aria-labelledby="scheduleModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="scheduleModalLabel">Edit Schedule</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <form id="schedule-edit-form">
                        <input type="hidden" id="edit-employee">
                        <input type="hidden" id="edit-day">
                        
                        <div class="mb-3">
                            <div class="form-check">
                                <input class="form-check-input" type="radio" name="schedule-type" id="schedule-working" value="working" checked>
                                <label class="form-check-label" for="schedule-working">
                                    Working
                                </label>
                            </div>
                            <div class="form-check">
                                <input class="form-check-input" type="radio" name="schedule-type" id="schedule-off" value="off">
                                <label class="form-check-label" for="schedule-off">
                                    Day Off
                                </label>
                            </div>
                        </div>
                        
                        <div id="working-hours">
                            <div class="row mb-3">
                                <div class="col">
                                    <label for="start-time" class="form-label">Start Time</label>
                                    <input type="time" class="form-control" id="start-time">
                                </div>
                                <div class="col">
                                    <label for="end-time" class="form-label">End Time</label>
                                    <input type="time" class="form-control" id="end-time">
                                </div>
                            </div>
                            
                            <div class="mb-3">
                                <label for="position-select" class="form-label">Position</label>
                                <select class="form-select" id="position-select">
                                    <option value="server">Server</option>
                                    <option value="bartender">Bartender</option>
                                    <option value="host">Host/Hostess</option>
                                    <option value="cook">Cook</option>
                                    <option value="dishwasher">Dishwasher</option>
                                    <option value="manager">Manager</option>
                                </select>
                            </div>
                            
                            <div class="mb-3">
                                <label for="notes" class="form-label">Notes</label>
                                <textarea class="form-control" id="notes" rows="3"></textarea>
                            </div>
                        </div>
                    </form>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
                    <button type="button" class="btn btn-primary" id="save-schedule">Save Changes</button>
                </div>
            </div>
        </div>
    </div>
    
    <!-- Availability Modal -->
    <div class="modal fade" id="availabilityModal" tabindex="-1" aria-labelledby="availabilityModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="availabilityModalLabel">Employee Availability</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <form id="availability-form">
                        <div class="row mb-4">
                            <div class="col-md-3">
                                <h6>Day</h6>
                            </div>
                            <div class="col-md-3">
                                <h6>Available</h6>
                            </div>
                            <div class="col-md-3">
                                <h6>From</h6>
                            </div>
                            <div class="col-md-3">
                                <h6>To</h6>
                            </div>
                        </div>
                        
                        <div class="row mb-3">
                            <div class="col-md-3">Monday</div>
                            <div class="col-md-3">
                                <div class="form-check">
                                    <input class="form-check-input availability-toggle" type="checkbox" id="monday-available" data-day="monday" checked>
                                    <label class="form-check-label" for="monday-available">Available</label>
                                </div>
                            </div>
                            <div class="col-md-3">
                                <input type="time" class="form-control" id="monday-from" value="09:00">
                            </div>
                            <div class="col-md-3">
                                <input type="time" class="form-control" id="monday-to" value="17:00">
                            </div>
                        </div>
                        
                        <!-- Repeat for other days -->
                        <div class="row mb-3">
                            <div class="col-md-3">Tuesday</div>
                            <div class="col-md-3">
                                <div class="form-check">
                                    <input class="form-check-input availability-toggle" type="checkbox" id="tuesday-available" data-day="tuesday" checked>
                                    <label class="form-check-label" for="tuesday-available">Available</label>
                                </div>
                            </div>
                            <div class="col-md-3">
                                <input type="time" class="form-control" id="tuesday-from" value="09:00">
                            </div>
                            <div class="col-md-3">
                                <input type="time" class="form-control" id="tuesday-to" value="17:00">
                            </div>
                        </div>
                        
                        <!-- Continue for other days -->
                        <div class="row mb-3">
                            <div class="col-md-3">Wednesday</div>
                            <div class="col-md-3">
                                <div class="form-check">
                                    <input class="form-check-input availability-toggle" type="checkbox" id="wednesday-available" data-day="wednesday" checked>
                                    <label class="form-check-label" for="wednesday-available">Available</label>
                                </div>
                            </div>
                            <div class="col-md-3">
                                <input type="time" class="form-control" id="wednesday-from" value="09:00">
                            </div>
                            <div class="col-md-3">
                                <input type="time" class="form-control" id="wednesday-to" value="17:00">
                            </div>
                        </div>
                        
                        <div class="row mb-3">
                            <div class="col-md-3">Thursday</div>
                            <div class="col-md-3">
                                <div class="form-check">
                                    <input class="form-check-input availability-toggle" type="checkbox" id="thursday-available" data-day="thursday" checked>
                                    <label class="form-check-label" for="thursday-available">Available</label>
                                </div>
                            </div>
                            <div class="col-md-3">
                                <input type="time" class="form-control" id="thursday-from" value="09:00">
                            </div>
                            <div class="col-md-3">
                                <input type="time" class="form-control" id="thursday-to" value="17:00">
                            </div>
                        </div>
                        
                        <div class="row mb-3">
                            <div class="col-md-3">Friday</div>
                            <div class="col-md-3">
                                <div class="form-check">
                                    <input class="form-check-input availability-toggle" type="checkbox" id="friday-available" data-day="friday" checked>
                                    <label class="form-check-label" for="friday-available">Available</label>
                                </div>
                            </div>
                            <div class="col-md-3">
                                <input type="time" class="form-control" id="friday-from" value="09:00">
                            </div>
                            <div class="col-md-3">
                                <input type="time" class="form-control" id="friday-to" value="17:00">
                            </div>
                        </div>
                        
                        <div class="row mb-3">
                            <div class="col-md-3">Saturday</div>
                            <div class="col-md-3">
                                <div class="form-check">
                                    <input class="form-check-input availability-toggle" type="checkbox" id="saturday-available" data-day="saturday" checked>
                                    <label class="form-check-label" for="saturday-available">Available</label>
                                </div>
                            </div>
                            <div class="col-md-3">
                                <input type="time" class="form-control" id="saturday-from" value="09:00">
                            </div>
                            <div class="col-md-3">
                                <input type="time" class="form-control" id="saturday-to" value="17:00">
                            </div>
                        </div>
                        
                        <div class="row mb-3">
                            <div class="col-md-3">Sunday</div>
                            <div class="col-md-3">
                                <div class="form-check">
                                    <input class="form-check-input availability-toggle" type="checkbox" id="sunday-available" data-day="sunday">
                                    <label class="form-check-label" for="sunday-available">Available</label>
                                </div>
                            </div>
                            <div class="col-md-3">
                                <input type="time" class="form-control" id="sunday-from" value="09:00" disabled>
                            </div>
                            <div class="col-md-3">
                                <input type="time" class="form-control" id="sunday-to" value="17:00" disabled>
                            </div>
                        </div>
                    </form>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
                    <button type="button" class="btn btn-primary" id="save-availability">Save Availability</button>
                </div>
            </div>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
    <script src="https://kit.fontawesome.com/a076d05399.js" crossorigin="anonymous"></script>
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Initialize tooltips and popovers
            const tooltipTriggerList = [].slice.call(document.querySelectorAll('[data-bs-toggle="tooltip"]'));
            tooltipTriggerList.map(function (tooltipTriggerEl) {
                return new bootstrap.Tooltip(tooltipTriggerEl);
            });
            
            // Schedule cell click handler
            const scheduleCells = document.querySelectorAll('.schedule-cell');
            scheduleCells.forEach(cell => {
                cell.addEventListener('click', function() {
                    const employee = this.dataset.employee;
                    const day = this.dataset.day;
                    const scheduleText = this.textContent.trim();
                    
                    document.getElementById('edit-employee').value = employee;
                    document.getElementById('edit-day').value = day;
                    
                    if (scheduleText === 'Off') {
                        document.getElementById('schedule-off').checked = true;
                        document.getElementById('working-hours').style.display = 'none';
                    } else {
                        document.getElementById('schedule-working').checked = true;
                        document.getElementById('working-hours').style.display = 'block';
                        
                        if (scheduleText.includes('-')) {
                            const times = scheduleText.split('-');
                            document.getElementById('start-time').value = convertTimeFormat(times[0].trim());
                            document.getElementById('end-time').value = convertTimeFormat(times[1].trim());
                        }
                    }
                    
                    const scheduleModal = new bootstrap.Modal(document.getElementById('scheduleModal'));
                    scheduleModal.show();
                });
            });
            
            // Schedule type radio button change handler
            document.querySelectorAll('input[name="schedule-type"]').forEach(radio => {
                radio.addEventListener('change', function() {
                    if (this.value === 'working') {
                        document.getElementById('working-hours').style.display = 'block';
                    } else {
                        document.getElementById('working-hours').style.display = 'none';
                    }
                });
            });
            
            // Save schedule button click handler
            document.getElementById('save-schedule').addEventListener('click', function() {
                const employee = document.getElementById('edit-employee').value;
                const day = document.getElementById('edit-day').value;
                const scheduleType = document.querySelector('input[name="schedule-type"]:checked').value;
                
                const cell = document.querySelector(`.schedule-cell[data-employee="${employee}"][data-day="${day}"]`);
                
                if (scheduleType === 'off') {
                    cell.textContent = 'Off';
                } else {
                    const startTime = document.getElementById('start-time').value;
                    const endTime = document.getElementById('end-time').value;
                    
                    if (startTime && endTime) {
                        cell.textContent = `${formatTimeDisplay(startTime)} - ${formatTimeDisplay(endTime)}`;
                    }
                }
                
                // Update overtime indicator
                updateOvertimeIndicator(employee);
                
                const scheduleModal = bootstrap.Modal.getInstance(document.getElementById('scheduleModal'));
                scheduleModal.hide();
            });
            
            // Availability toggle change handler
            document.querySelectorAll('.availability-toggle').forEach(toggle => {
                toggle.addEventListener('change', function() {
                    const day = this.dataset.day;
                    const fromInput = document.getElementById(`${day}-from`);
                    const toInput = document.getElementById(`${day}-to`);
                    
                    if (this.checked) {
                        fromInput.disabled = false;
                        toInput.disabled = false;
                    } else {
                        fromInput.disabled = true;
                        toInput.disabled = true;
                    }
                });
            });
            
            // PDF dropzone click handler
            document.getElementById('pdf-dropzone').addEventListener('click', function() {
                document.getElementById('pdf-upload').click();
            });
            
            // PDF file input change handler
            document.getElementById('pdf-upload').addEventListener('change', function(e) {
                handleFiles(e.target.files);
            });
            
            // PDF dropzone drag and drop handlers
            const dropzone = document.getElementById('pdf-dropzone');
            
            dropzone.addEventListener('dragover', function(e) {
                e.preventDefault();
                e.stopPropagation();
                this.classList.add('border-primary');
            });
            
            dropzone.addEventListener('dragleave', function(e) {
                e.preventDefault();
                e.stopPropagation();
                this.classList.remove('border-primary');
            });
            
            dropzone.addEventListener('drop', function(e) {
                e.preventDefault();
                e.stopPropagation();
                this.classList.remove('border-primary');
                
                const files = e.dataTransfer.files;
                handleFiles(files);
            });
            
            // Generate schedule button click handler
            document.getElementById('generate-schedule').addEventListener('click', function() {
                // Simulate AI-generated schedule
                alert('Generating optimal schedule using DeepSeek R1 API...');
                setTimeout(() => {
                    alert('Schedule generated successfully!');
                }, 2000);
            });
            
            // Share buttons click handlers
            document.getElementById('share-email').addEventListener('click', function(e) {
                e.preventDefault();
                alert('Schedule sent via email to all employees!');
            });
            
            document.getElementById('share-text').addEventListener('click', function(e) {
                e.preventDefault();
                alert('Schedule sent via text message to all employees!');
            });
            
            document.getElementById('share-whatsapp').addEventListener('click', function(e) {
                e.preventDefault();
                alert('Schedule shared to WhatsApp group!');
            });
            
            // Add employee form submit handler
            document.getElementById('employee-form').addEventListener('submit', function(e) {
                e.preventDefault();
                
                const name = document.getElementById('employee-name').value;
                const position = document.getElementById('employee-position').value;
                const contact = document.getElementById('employee-contact').value;
                
                if (name && position && contact) {
                    addEmployeeToSchedule(name, position);
                    this.reset();
                    alert(`Employee ${name} added successfully!`);
                }
            });
            
            // Helper functions
            function convertTimeFormat(timeStr) {
                // Convert from display format (10:00) to input format (10:00)
                if (timeStr.includes(':')) {
                    const [hours, minutes] = timeStr.split(':');
                    return `${hours.padStart(2, '0')}:${minutes.padStart(2, '0')}`;
                }
                return '';
            }
            
            function formatTimeDisplay(timeStr) {
                // Convert from input format (10:00) to display format (10:00)


