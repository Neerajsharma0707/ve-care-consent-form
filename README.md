# ve-care-consent-form
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>High-Risk ICU Patient Transfer Consent Form</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <!-- jsPDF and html2canvas for PDF generation -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

    <style>
        body {
             font-family: 'Inter', sans-serif;
            background-color: #f0f2f5;
            color: #333;
            line-height: 1.6;
        }
        .container {
            max-width: 900px;
            margin: 2rem auto;
            padding: 2rem;
            background-color: #fff;
            border-radius: 12px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
            border: 1px solid #e0e0e0;
        }
        h1, h2 {
            color: #1a202c;
            font-weight: 700;
        }
        .form-section {
            margin-bottom: 2rem;
            padding: 1.5rem;
            border: 1px solid #e2e8f0;
            border-radius: 8px;
            background-color: #f8fafc;
        }
        .form-group label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: 500;
            color: #4a5568;
        }
        .form-group input[type="text"],
        .form-group input[type="number"],
        .form-group input[type="date"],
        .form-group input[type="time"],
        .form-group textarea,
        .form-group select {
            width: 100%;
            padding: 0.75rem 1rem;
            border: 1px solid #cbd5e0;
            border-radius: 6px;
            background-color: #ffffff;
            transition: border-color 0.2s ease-in-out, box-shadow 0.2s ease-in-out;
            font-size: 1rem;
        }
        .form-group input:focus,
        .form-group textarea:focus,
        .form-group select:focus {
            outline: none;
            border-color: #4c51bf;
            box-shadow: 0 0 0 3px rgba(76, 81, 191, 0.2);
        }
        .form-group textarea {
            min-height: 80px;
            resize: vertical;
        }
        .consent-checkbox {
            margin-top: 1.5rem;
        }
        .consent-checkbox input[type="checkbox"] {
            margin-right: 0.75rem;
            width: 1.25rem;
            height: 1.25rem;
            border-radius: 4px;
            border: 1px solid #cbd5e0;
            accent-color: #4c51bf; /* For checked state */
        }
        .info-block {
            background-color: #e0f2fe;
            border-left: 5px solid #38b2ac;
            padding: 1rem;
            border-radius: 8px;
            margin-bottom: 1rem;
            color: #2c5282;
            font-size: 0.95rem;
        }
        .signature-pad {
            border: 2px solid #4c51bf;
            border-radius: 8px;
            cursor: crosshair;
            background-color: #ffffff;
        }
        .btn {
            padding: 0.75rem 1.5rem;
            border-radius: 8px;
            font-weight: 600;
            cursor: pointer;
            transition: background-color 0.2s ease-in-out, box-shadow 0.2s ease-in-out;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }
        .btn-primary {
            background-color: #4c51bf;
            color: #fff;
        }
        .btn-primary:hover {
            background-color: #5a64d1;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
        }
        .btn-secondary {
            background-color: #edf2f7;
            color: #4a5568;
            border: 1px solid #cbd5e0;
        }
        .btn-secondary:hover {
            background-color: #e2e8f0;
        }
        .grid-cols-2 {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 1.5rem;
        }
        .grid-cols-3 {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 1.5rem;
        }
        /* Responsive adjustments */
        @media (max-width: 768px) {
            .container {
                margin: 1rem;
                padding: 1rem;
            }
            .grid-cols-2, .grid-cols-3 {
                grid-template-columns: 1fr;
            }
        }
        .loader {
            border: 4px solid #f3f3f3; /* Light grey */
            border-top: 4px solid #3498db; /* Blue */
            border-radius: 50%;
            width: 20px;
            height: 20px;
            animation: spin 2s linear infinite;
            display: inline-block;
            vertical-align: middle;
            margin-left: 10px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        .summary-box {
            background-color: #e6fffa;
            border: 1px solid #81e6d9;
            border-radius: 8px;
            padding: 1.5rem;
            margin-top: 1.5rem;
            color: #234e52;
            font-size: 1rem;
            line-height: 1.6;
            max-height: 300px;
            overflow-y: auto;
            text-align: left;
        }
        .summary-box h3 {
            font-weight: 600;
            margin-bottom: 0.75rem;
            color: #2d3748;
        }
    </style>
</head>
<body>
    <div class="container" id="consentFormContainer">
        <h1 class="text-3xl font-bold text-center mb-6">High-Risk ICU Patient Transfer Consent Form</h1>
        <p class="text-center text-gray-600 mb-8">This form is for obtaining consent for the transportation of a high-risk ICU patient via mobile ICU ambulance (ICU on Wheels) provided by Ve Care Health Care Services.</p>

        <!-- Section 1: Patient Details -->
        <div class="form-section">
            <h2 class="text-2xl mb-4">1. Patient Details</h2>
            <div class="grid-cols-2">
                <div class="form-group">
                    <label for="patientName">Name:</label>
                    <input type="text" id="patientName" name="patientName" required class="rounded-md">
                </div>
                <div class="form-group">
                    <label for="patientAgeSex">Age/Sex:</label>
                    <input type="text" id="patientAgeSex" name="patientAgeSex" placeholder="e.g., 65/Male" class="rounded-md">
                </div>
            </div>
            <div class="form-group mt-4">
                <label for="diagnosis">Diagnosis:</label>
                <textarea id="diagnosis" name="diagnosis" class="rounded-md"></textarea>
            </div>
            <div class="form-group mt-4">
                <label for="currentCondition">Current Condition:</label>
                <textarea id="currentCondition" name="currentCondition" class="rounded-md"></textarea>
            </div>
            <div class="grid-cols-2 mt-4">
                <div class="form-group">
                    <label for="referredFromHospital">Referred From Hospital:</label>
                    <input type="text" id="referredFromHospital" name="referredFromHospital" class="rounded-md">
                </div>
                <div class="form-group">
                    <label for="referredToHospital">Referred To Hospital:</label>
                    <input type="text" id="referredToHospital" name="referredToHospital" class="rounded-md">
                </div>
            </div>
            <div class="grid-cols-2 mt-4">
                <div class="form-group">
                    <label for="transferDate">Date of Transfer:</label>
                    <input type="date" id="transferDate" name="transferDate" class="rounded-md">
                </div>
                <div class="form-group">
                    <label for="transferTime">Time of Transfer:</label>
                    <input type="time" id="transferTime" name="transferTime" class="rounded-md">
                </div>
            </div>
        </div>

        <!-- Section 2: Patient Vital Signs -->
        <div class="form-section">
            <h2 class="text-2xl mb-4">2. Patient Vital Signs (Optional)</h2>
            <p class="text-gray-600 mb-4">Please fill this section if applicable.</p>

            <h3 class="text-xl font-semibold mb-3">At time of pickup:</h3>
            <div class="grid-cols-3">
                <div class="form-group">
                    <label for="pickupBP">BP:</label>
                    <input type="text" id="pickupBP" class="rounded-md">
                </div>
                <div class="form-group">
                    <label for="pickupPulse">Pulse:</label>
                    <input type="text" id="pickupPulse" class="rounded-md">
                </div>
                <div class="form-group">
                    <label for="pickupSPO2">SPO2:</label>
                    <input type="text" id="pickupSPO2" class="rounded-md">
                </div>
                <div class="form-group">
                    <label for="pickupRR">RR:</label>
                    <input type="text" id="pickupRR" class="rounded-md">
                </div>
                <div class="form-group">
                    <label for="pickupTemp">Temp:</label>
                    <input type="text" id="pickupTemp" class="rounded-md">
                </div>
                <div class="form-group">
                    <label for="pickupGCS">GCS:</label>
                    <input type="text" id="pickupGCS" class="rounded-md">
                </div>
            </div>
            <div class="form-group mt-4">
                <label for="pickupRemarks">Remarks:</label>
                <textarea id="pickupRemarks" class="rounded-md"></textarea>
            </div>

            <h3 class="text-xl font-semibold mt-6 mb-3">During transport (midway):</h3>
            <div class="grid-cols-3">
                <div class="form-group">
                    <label for="midwayBP">BP:</label>
                    <input type="text" id="midwayBP" class="rounded-md">
                </div>
                <div class="form-group">
                    <label for="midwayPulse">Pulse:</label>
                    <input type="text" id="midwayPulse" class="rounded-md">
                </div>
                <div class="form-group">
                    <label for="midwaySPO2">SPO2:</label>
                    <input type="text" id="midwaySPO2" class="rounded-md">
                </div>
                <div class="form-group">
                    <label for="midwayRR">RR:</label>
                    <input type="text" id="midwayRR" class="rounded-md">
                </div>
                <div class="form-group">
                    <label for="midwayTemp">Temp:</label>
                    <input type="text" id="midwayTemp" class="rounded-md">
                </div>
                <div class="form-group">
                    <label for="midwayGCS">GCS:</label>
                    <input type="text" id="midwayGCS" class="rounded-md">
                </div>
            </div>
            <div class="form-group mt-4">
                <label for="midwayRemarks">Remarks:</label>
                <textarea id="midwayRemarks" class="rounded-md"></textarea>
            </div>

            <h3 class="text-xl font-semibold mt-6 mb-3">At handover to hospital:</h3>
            <div class="grid-cols-3">
                <div class="form-group">
                    <label for="handoverBP">BP:</label>
                    <input type="text" id="handoverBP" class="rounded-md">
                </div>
                <div class="form-group">
                    <label for="handoverPulse">Pulse:</label>
                    <input type="text" id="handoverPulse" class="rounded-md">
                </div>
                <div class="form-group">
                    <label for="handoverSPO2">SPO2:</label>
                    <input type="text" id="handoverSPO2" class="rounded-md">
                </div>
                <div class="form-group">
                    <label for="handoverRR">RR:</label>
                    <input type="text" id="handoverRR" class="rounded-md">
                </div>
                <div class="form-group">
                    <label for="handoverTemp">Temp:</label>
                    <input type="text" id="handoverTemp" class="rounded-md">
                </div>
                <div class="form-group">
                    <label for="handoverGCS">GCS:</label>
                    <input type="text" id="handoverGCS" class="rounded-md">
                </div>
            </div>
            <div class="form-group mt-4">
                <label for="handoverRemarks">Remarks:</label>
                <textarea id="handoverRemarks" class="rounded-md"></textarea>
            </div>
        </div>

        <!-- Section 3: Declaration by Patient's Relative / Legal Guardian -->
        <div class="form-section">
            <h2 class="text-2xl mb-4">3. Declaration by Patient's Relative / Legal Guardian</h2>
            <div class="form-group">
                <label for="relativeName">I, Mr./Ms.:</label>
                <input type="text" id="relativeName" name="relativeName" required class="rounded-md">
            </div>
            <div class="form-group mt-4">
                <label for="relativeRelation">Relation to Patient:</label>
                <input type="text" id="relativeRelation" name="relativeRelation" required class="rounded-md">
            </div>

            <div class="consent-checkbox flex items-start">
                <input type="checkbox" id="consentDeclaration" name="consentDeclaration" class="rounded-md" required>
                <label for="consentDeclaration" class="ml-2 cursor-pointer text-gray-700">
                    hereby give my full consent for the transportation of the above-named patient in a mobile ICU ambulance (ICU on Wheels) provided by Ve Care Health Care Services.
                </label>
            </div>

            <h3 class="text-xl font-semibold mt-8 mb-3">I have been explained that:</h3>
            <div class="info-block">
                <p>• The patient is in critical condition and transfer is being done on the recommendation of the referring doctor.</p>
                <p>• There is a risk of worsening of condition or death during transit despite precautions.</p>
                <p>• Ve Care is a transport service, not a treating hospital, and is not liable for any medical outcome including death.</p>
                <p>• The onboard medical team is for basic ICU care support during transit, not full hospital treatment.</p>
            </div>
        </div>

        <!-- Section 4: Consent & Waiver -->
        <div class="form-section">
            <h2 class="text-2xl mb-4">4. Consent & Waiver</h2>
            <div class="consent-checkbox flex items-start">
                <input type="checkbox" id="consentWaiver" name="consentWaiver" class="rounded-md" required>
                <label for="consentWaiver" class="ml-2 cursor-pointer text-gray-700">
                    I accept all risks and do not hold Ve Care, its staff, or associated doctors responsible for any complications or death that may occur during transfer.
                </label>
            </div>
        </div>

        <!-- Section 5: Signature of Relative / Legal Guardian Details -->
        <div class="form-section">
            <h2 class="text-2xl mb-4">5. Signature of Relative / Legal Guardian</h2>
            <p class="text-gray-600 mb-4">Please provide your electronic signature below.</p>
            <div class="form-group mb-4">
                <label for="relativeSignatureName">Full Name (Typed as Electronic Signature):</label>
                <input type="text" id="relativeSignatureName" name="relativeSignatureName" required class="rounded-md">
                <p class="text-sm text-gray-500 mt-1">By typing your full name above, you are providing your electronic signature.</p>
            </div>
            <div class="form-group mb-4">
                <label for="relativeIDProof">ID Proof:</label>
                <input type="text" id="relativeIDProof" name="relativeIDProof" placeholder="e.g., Aadhar Number or Identity Number" class="rounded-md">
            </div>
            <div class="grid-cols-2">
                <div class="form-group">
                    <label for="relativeSignDate">Date:</label>
                    <input type="date" id="relativeSignDate" name="relativeSignDate" class="rounded-md">
                </div>
                <div class="form-group">
                    <label for="relativeSignTime">Time:</label>
                    <input type="time" id="relativeSignTime" name="relativeSignTime" class="rounded-md">
                </div>
            </div>
            <div class="form-group mt-4">
                <label for="relativeSignContact">Contact Number:</label>
                <input type="text" id="relativeSignContact" name="relativeSignContact" class="rounded-md">
            </div>

            <p class="text-gray-600 mt-6 mb-3">Alternatively, draw your signature below:</p>
            <canvas id="signatureCanvasRelative" class="signature-pad" width="400" height="150"></canvas>
            <div class="mt-4 flex gap-4">
                <button type="button" id="clearSignatureRelative" class="btn btn-secondary">Clear Signature</button>
            </div>
            <input type="hidden" id="relativeSignatureData" name="relativeSignatureData">
        </div>

        <!-- Section 6: Signature of Attending Ambulance Staff -->
        <div class="form-section">
            <h2 class="text-2xl mb-4">6. Signature of Attending Ambulance Staff (Paramedic/Doctor)</h2>
            <div class="form-group">
                <label for="staffName">Name:</label>
                <input type="text" id="staffName" name="staffName" class="rounded-md">
            </div>
            <div class="form-group mt-4">
                <label for="staffDesignation">Designation:</label>
                <input type="text" id="staffDesignation" name="staffDesignation" class="rounded-md">
            </div>
            <div class="grid-cols-2 mt-4">
                <div class="form-group">
                    <label for="staffSignDate">Date:</label>
                    <input type="date" id="staffSignDate" name="staffSignDate" class="rounded-md">
                </div>
                <div class="form-group">
                    <label for="staffSignTime">Time:</label>
                    <input type="time" id="staffSignTime" name="staffSignTime" class="rounded-md">
                </div>
            </div>
            <p class="text-gray-600 mt-6 mb-3">Draw Staff's Signature:</p>
            <canvas id="signatureCanvasStaff" class="signature-pad" width="400" height="150"></canvas>
            <div class="mt-4 flex gap-4">
                <button type="button" id="clearSignatureStaff" class="btn btn-secondary">Clear Signature</button>
            </div>
            <input type="hidden" id="staffSignatureData" name="staffSignatureData">
        </div>

        <div class="mt-8 text-center flex flex-col sm:flex-row justify-center items-center gap-4">
            <button type="submit" class="btn btn-primary" id="submitFormBtn">Submit Consent Form</button>
            <button type="button" class="btn btn-secondary" id="generatePdfBtn">Generate PDF for Sharing <span id="pdfLoader" class="loader hidden"></span></button>
            <button type="button" class="btn btn-primary" id="summarizeFormBtn">Summarize Form Data ✨ <span id="summaryLoader" class="loader hidden"></span></button>
        </div>

        <!-- LLM Summary Display Area -->
        <div id="summaryDisplay" class="summary-box hidden">
            <h3>Form Data Summary:</h3>
            <p id="summaryContent"></p>
        </div>
    </div>

    <script>
        // Signature pad logic
        function setupSignaturePad(canvasId, hiddenInputId) {
            const canvas = document.getElementById(canvasId);
            const ctx = canvas.getContext('2d');
            const hiddenInput = document.getElementById(hiddenInputId);

            let drawing = false;

            // Set canvas size for responsiveness, respecting aspect ratio
            function resizeCanvas() {
                const parent = canvas.parentElement;
                const ratio = window.devicePixelRatio || 1;
                // Use a fixed width relative to parent for calculation, then scale
                // Ensure max width is respected, but allow it to shrink
                const desiredWidth = Math.min(parent.clientWidth, 400); // Max 400px, or parent width
                canvas.width = desiredWidth * ratio;
                canvas.height = desiredWidth * (150 / 400) * ratio; // Maintain aspect ratio
                ctx.scale(ratio, ratio);
                ctx.lineWidth = 2;
                ctx.lineCap = 'round';
                ctx.strokeStyle = '#000';
            }

            resizeCanvas();
            window.addEventListener('resize', resizeCanvas);

            function getMousePos(e) {
                const rect = canvas.getBoundingClientRect();
                return {
                    x: (e.clientX - rect.left) / (rect.width / canvas.width * (window.devicePixelRatio || 1)),
                    y: (e.clientY - rect.top) / (rect.height / canvas.height * (window.devicePixelRatio || 1))
                };
            }

            function getTouchPos(e) {
                const rect = canvas.getBoundingClientRect();
                return {
                    x: (e.touches[0].clientX - rect.left) / (rect.width / canvas.width * (window.devicePixelRatio || 1)),
                    y: (e.touches[0].clientY - rect.top) / (rect.height / canvas.height * (window.devicePixelRatio || 1))
                };
            }

            function startPosition(e) {
                drawing = true;
                ctx.beginPath();
                const pos = e.type.includes('mouse') ? getMousePos(e) : getTouchPos(e);
                ctx.moveTo(pos.x, pos.y);
            }

            function draw(e) {
                if (!drawing) return;
                e.preventDefault(); // Prevent scrolling on touch devices when drawing
                const pos = e.type.includes('mouse') ? getMousePos(e) : getTouchPos(e);
                ctx.lineTo(pos.x, pos.y);
                ctx.stroke();
            }

            function endPosition() {
                drawing = false;
                // Save the signature as a data URL when drawing ends
                hiddenInput.value = canvas.toDataURL();
            }

            // Mouse events
            canvas.addEventListener('mousedown', startPosition);
            canvas.addEventListener('mouseup', endPosition);
            canvas.addEventListener('mouseout', endPosition); // Stop drawing if mouse leaves canvas
            canvas.addEventListener('mousemove', draw);

            // Touch events
            canvas.addEventListener('touchstart', startPosition, { passive: false }); // Use passive: false for preventDefault
            canvas.addEventListener('touchend', endPosition);
            canvas.addEventListener('touchcancel', endPosition); // Stop drawing if touch is cancelled
            canvas.addEventListener('touchmove', draw, { passive: false }); // Use passive: false for preventDefault

            // Clear button functionality
            document.getElementById(`clearSignature${canvasId.replace('signatureCanvas', '')}`).addEventListener('click', () => {
                ctx.clearRect(0, 0, canvas.width, canvas.height);
                hiddenInput.value = ''; // Clear the hidden input data
            });
        }

        // Setup signature pads for each relevant section
        window.onload = function() {
            setupSignaturePad('signatureCanvasRelative', 'relativeSignatureData');
            setupSignaturePad('signatureCanvasStaff', 'staffSignatureData');

            // Auto-fill current date and time for relevant fields on load
            const today = new Date();
            const date = today.toISOString().split('T')[0];
            const time = today.toTimeString().split(' ')[0].substring(0, 5);

            document.getElementById('relativeSignDate').value = date;
            document.getElementById('relativeSignTime').value = time;
            document.getElementById('staffSignDate').value = date;
            document.getElementById('staffSignTime').value = time;
        };

        // Custom message box utility function
        function showMessageBox(message, type = 'success') {
            const existingMessageBox = document.querySelector('.message-box');
            if (existingMessageBox) existingMessageBox.remove(); // Remove any prior message box

            const messageBox = document.createElement('div');
            messageBox.className = 'message-box'; // Add a class for potential styling
            messageBox.style.cssText = `
                position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%);
                padding: 20px; border-radius: 8px;
                box-shadow: 0 4px 8px rgba(0,0,0,0.2); z-index: 1000; text-align: center;
                font-size: 1.1rem;
            `;

            if (type === 'success') {
                messageBox.style.backgroundColor = '#4CAF50';
                messageBox.style.color = 'white';
            } else if (type === 'info') {
                messageBox.style.backgroundColor = '#2196F3';
                messageBox.style.color = 'white';
            } else if (type === 'error') {
                messageBox.style.backgroundColor = '#f44336';
                messageBox.style.color = 'white';
            }

            messageBox.innerHTML = `
                <p>${message}</p>
                <button onclick="this.parentNode.remove()" style="background-color: white; color: ${messageBox.style.backgroundColor}; border: none; padding: 8px 15px; border-radius: 5px; cursor: pointer; margin-top: 15px;">OK</button>
            `;
            document.body.appendChild(messageBox);
        }


        // Basic form submission handler (for demonstration)
        document.getElementById('submitFormBtn').addEventListener('click', (event) => {
            event.preventDefault(); // Prevent actual form submission for this prototype

            // You would normally validate the form here before gathering data
            // For now, just gather data
            const formData = {
                patientDetails: {
                    name: document.getElementById('patientName').value,
                    ageSex: document.getElementById('patientAgeSex').value,
                    diagnosis: document.getElementById('diagnosis').value,
                    currentCondition: document.getElementById('currentCondition').value,
                    referredFromHospital: document.getElementById('referredFromHospital').value,
                    referredToHospital: document.getElementById('referredToHospital').value,
                    transferDate: document.getElementById('transferDate').value,
                    transferTime: document.getElementById('transferTime').value,
                },
                vitalSigns: {
                    pickup: {
                        bp: document.getElementById('pickupBP').value, pulse: document.getElementById('pickupPulse').value,
                        spo2: document.getElementById('pickupSPO2').value, rr: document.getElementById('pickupRR').value,
                        temp: document.getElementById('pickupTemp').value, gcs: document.getElementById('pickupGCS').value,
                        remarks: document.getElementById('pickupRemarks').value,
                    },
                    midway: {
                        bp: document.getElementById('midwayBP').value, pulse: document.getElementById('midwayPulse').value,
                        spo2: document.getElementById('midwaySPO2').value, rr: document.getElementById('midwayRR').value,
                        temp: document.getElementById('midwayTemp').value, gcs: document.getElementById('midwayGCS').value,
                        remarks: document.getElementById('midwayRemarks').value,
                    },
                    handover: {
                        bp: document.getElementById('handoverBP').value, pulse: document.getElementById('handoverPulse').value,
                        spo2: document.getElementById('handoverSPO2').value, rr: document.getElementById('handoverRR').value,
                        temp: document.getElementById('handoverTemp').value, gcs: document.getElementById('handoverGCS').value,
                        remarks: document.getElementById('handoverRemarks').value,
                    }
                },
                relativeDeclaration: {
                    name: document.getElementById('relativeName').value,
                    relation: document.getElementById('relativeRelation').value,
                    consentChecked: document.getElementById('consentDeclaration').checked,
                },
                consentWaiver: document.getElementById('consentWaiver').checked,
                relativeSignature: {
                    typedName: document.getElementById('relativeSignatureName').value,
                    idProof: document.getElementById('relativeIDProof').value,
                    date: document.getElementById('relativeSignDate').value,
                    time: document.getElementById('relativeSignTime').value,
                    contact: document.getElementById('relativeSignContact').value,
                    drawnSignature: document.getElementById('relativeSignatureData').value, // This holds the base64 image
                },
                staffSignature: {
                    name: document.getElementById('staffName').value,
                    designation: document.getElementById('staffDesignation').value,
                    date: document.getElementById('staffSignDate').value,
                    time: document.getElementById('staffSignTime').value,
                    drawnSignature: document.getElementById('staffSignatureData').value,
                }
            };

            console.log("Form Data:", formData);
            showMessageBox("Form submitted! (Data logged to console. This is a demo; data is not actually saved.)", 'success');
        });

        // PDF Generation Logic
        document.getElementById('generatePdfBtn').addEventListener('click', async () => {
            const btn = document.getElementById('generatePdfBtn');
            const loader = document.getElementById('pdfLoader');

            btn.disabled = true; // Disable button during PDF generation
            loader.classList.remove('hidden'); // Show loader

            const formContainer = document.getElementById('consentFormContainer');

            // Temporarily hide buttons that shouldn't appear in the PDF
            const buttons = formContainer.querySelectorAll('.btn');
            buttons.forEach(button => button.style.display = 'none');

            // Set a specific width for PDF rendering to control layout consistency
            const originalWidth = formContainer.style.width;
            const originalMargin = formContainer.style.margin;
            formContainer.style.width = '800px'; // A reasonable width for A4
            formContainer.style.margin = '0 auto'; // Center for capture

            try {
                // Generate canvas from HTML content
                const canvas = await html2canvas(formContainer, {
                    scale: 2, // Increase scale for better resolution
                    useCORS: true, // If you have images from other domains
                    logging: true, // For debugging
                });

                // Convert canvas to JPEG data URL with quality compression
                const imgData = canvas.toDataURL('image/jpeg', 0.9); // Use JPEG with 90% quality
                const { jsPDF } = window.jspdf;
                const pdf = new jsPDF('p', 'mm', 'a4'); // 'p' for portrait, 'mm' for millimeters, 'a4' size

                const imgWidth = 210; // A4 width in mm
                const pageHeight = 297; // A4 height in mm
                const imgHeight = (canvas.height * imgWidth) / canvas.width;
                let heightLeft = imgHeight;
                let position = 0;

                pdf.addImage(imgData, 'JPEG', 0, position, imgWidth, imgHeight); // Specify JPEG here
                heightLeft -= pageHeight;

                while (heightLeft >= 0) {
                    position = heightLeft - imgHeight;
                    pdf.addPage();
                    pdf.addImage(imgData, 'JPEG', 0, position, imgWidth, imgHeight); // Specify JPEG here
                    heightLeft -= pageHeight;
                }

                pdf.save('VeCare_Consent_Form.pdf');

                showMessageBox("PDF generated successfully! You can now download and share the PDF.", 'info');

            } catch (error) {
                console.error("Error generating PDF:", error);
                showMessageBox(`Error generating PDF. Please try again. Details: ${error.message}`, 'error');
            } finally {
                // Restore button visibility and form container width
                buttons.forEach(button => button.style.display = '');
                formContainer.style.width = originalWidth;
                formContainer.style.margin = originalMargin;
                btn.disabled = false; // Re-enable button
                loader.classList.add('hidden'); // Hide loader
            }
        });

        // LLM Summary Feature
        document.getElementById('summarizeFormBtn').addEventListener('click', async () => {
            const btn = document.getElementById('summarizeFormBtn');
            const loader = document.getElementById('summaryLoader');
            const summaryDisplay = document.getElementById('summaryDisplay');
            const summaryContent = document.getElementById('summaryContent');

            btn.disabled = true;
            loader.classList.remove('hidden');
            summaryDisplay.classList.add('hidden'); // Hide previous summary

            // Gather relevant form data for the summary
            const patientName = document.getElementById('patientName').value;
            const patientAgeSex = document.getElementById('patientAgeSex').value;
            const diagnosis = document.getElementById('diagnosis').value;
            const currentCondition = document.getElementById('currentCondition').value;
            const referredFromHospital = document.getElementById('referredFromHospital').value;
            const referredToHospital = document.getElementById('referredToHospital').value;
            const transferDate = document.getElementById('transferDate').value;
            const transferTime = document.getElementById('transferTime').value;
            const relativeName = document.getElementById('relativeName').value;
            const relativeRelation = document.getElementById('relativeRelation').value;
            const consentDeclarationChecked = document.getElementById('consentDeclaration').checked;
            const consentWaiverChecked = document.getElementById('consentWaiver').checked;
            const relativeIDProof = document.getElementById('relativeIDProof').value;
            const relativeContact = document.getElementById('relativeSignContact').value;
            const staffName = document.getElementById('staffName').value;
            const staffDesignation = document.getElementById('staffDesignation').value;


            // Construct the prompt for the LLM
            const prompt = `Summarize the following patient transfer consent form data concisely, focusing on key patient details, referral information, and the declarations made by the relative and staff. Do not interpret medical conditions or give medical advice.
            
Patient Details:
- Name: ${patientName || 'N/A'}
- Age/Sex: ${patientAgeSex || 'N/A'}
- Diagnosis: ${diagnosis || 'N/A'}
- Current Condition: ${currentCondition || 'N/A'}
- Referred From Hospital: ${referredFromHospital || 'N/A'}
- Referred To Hospital: ${referredToHospital || 'N/A'}
- Transfer Date & Time: ${transferDate || 'N/A'} at ${transferTime || 'N/A'}

Relative/Legal Guardian Declaration:
- Declarant Name: ${relativeName || 'N/A'}
- Relation to Patient: ${relativeRelation || 'N/A'}
- Consent Declaration Checked: ${consentDeclarationChecked ? 'Yes' : 'No'}
- Consent Waiver Checked: ${consentWaiverChecked ? 'Yes' : 'No'}
- ID Proof: ${relativeIDProof || 'N/A'}
- Contact Number: ${relativeContact || 'N/A'}

Attending Ambulance Staff:
- Name: ${staffName || 'N/A'}
- Designation: ${staffDesignation || 'N/A'}

Provide the summary as bullet points.`;

            let chatHistory = [];
            chatHistory.push({ role: "user", parts: [{ text: prompt }] });
            const payload = { contents: chatHistory };
            const apiKey = ""; // Canvas will automatically provide this at runtime
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                const result = await response.json();
                if (result.candidates && result.candidates.length > 0 &&
                    result.candidates[0].content && result.candidates[0].content.parts &&
                    result.candidates[0].content.parts.length > 0) {
                    const text = result.candidates[0].content.parts[0].text;
                    summaryContent.textContent = text;
                    summaryDisplay.classList.remove('hidden'); // Show the summary box
                } else {
                    console.error("LLM response structure unexpected:", result);
                    summaryContent.textContent = "Could not generate summary. Unexpected response from AI.";
                    summaryDisplay.classList.remove('hidden');
                }
            } catch (error) {
                console.error("Error calling Gemini API:", error);
                summaryContent.textContent = "Error generating summary. Please try again later.";
                summaryDisplay.classList.remove('hidden');
            } finally {
                btn.disabled = false;
                loader.classList.add('hidden');
            }
        });
    </script>
</body>
</html>
