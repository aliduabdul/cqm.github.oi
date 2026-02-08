<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Clinical Decision Support System - Pneumococcal Vaccine</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

         body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #0c1022 0%, #764ba2 100%);
            padding: 20px;
            min-height: 100vh;
        }
        
        .container {
            max-width: 1400px;
            margin: 0 auto;
        }
        
        h1 {
            text-align: center;
            color: white;
            margin-bottom: 30px;
            font-size: 2em;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }
        
        .dashboard-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(600px, 1fr));
            gap: 20px;
            margin-bottom: 20px;
        }
        
        .card {
            background: white;
            border-radius: 12px;
            padding: 25px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            transition: transform 0.3s ease;
        }
        
        .card:hover {
            transform: translateY(-5px);
        }
        
        .card-title {
            font-size: 1.5em;
            color: #333;
            margin-bottom: 20px;
            border-bottom: 3px solid #667eea;
            padding-bottom: 10px;
        }
        
        .ehr-screen {
            background: #f8f9fa;
            border: 2px solid #dee2e6;
            border-radius: 8px;
            padding: 20px;
            margin-top: 15px;
        }
        
        .patient-banner {
            background: #e3f2fd;
            padding: 15px;
            border-radius: 6px;
            margin-bottom: 15px;
            border-left: 4px solid #2196F3;
        }
        
        .patient-info {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            font-size: 0.9em;
        }
        
        .info-item {
            display: flex;
            flex-direction: column;
        }
        
        .info-label {
            font-weight: bold;
            color: #555;
            font-size: 0.85em;
        }
        
        .info-value {
            color: #1976D2;
            font-size: 1.1em;
            font-weight: 600;
        }
        
        .alert-box {
            background: #fff3cd;
            border: 2px solid #ffc107;
            border-left: 6px solid #ffc107;
            border-radius: 8px;
            padding: 20px;
            margin: 15px 0;
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0%, 100% { box-shadow: 0 0 0 0 rgba(255, 193, 7, 0.4); }
            50% { box-shadow: 0 0 0 10px rgba(255, 193, 7, 0); }
        }
        
        .alert-overdue {
            background: #ffebee;
            border-color: #f44336;
            border-left-color: #f44336;
        }
        
        .alert-header {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 10px;
        }
        
        .alert-icon {
            font-size: 1.5em;
        }
        
        .alert-title {
            font-weight: bold;
            font-size: 1.1em;
            color: #333;
        }
        
        .alert-message {
            margin: 10px 0;
            color: #555;
            line-height: 1.6;
        }
        
        .button-group {
            display: flex;
            gap: 10px;
            margin-top: 15px;
            flex-wrap: wrap;
        }
        
        .btn {
            padding: 10px 20px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s ease;
            font-size: 0.95em;
        }
        
        .btn-primary {
            background: #4CAF50;
            color: white;
        }
        
        .btn-primary:hover {
            background: #45a049;
            transform: scale(1.05);
        }
        
        .btn-secondary {
            background: #2196F3;
            color: white;
        }
        
        .btn-secondary:hover {
            background: #0b7dda;
        }
        
        .btn-warning {
            background: #ff9800;
            color: white;
        }
        
        .btn-warning:hover {
            background: #e68900;
        }
        
        .btn-danger {
            background: #f44336;
            color: white;
        }
        
        .data-flow {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin: 20px 0;
            flex-wrap: wrap;
            gap: 10px;
        }
        
        .flow-step {
            background: #e8eaf6;
            padding: 15px;
            border-radius: 8px;
            flex: 1;
            min-width: 150px;
            text-align: center;
            border: 2px solid #5c6bc0;
        }
        
        .flow-arrow {
            font-size: 2em;
            color: #5c6bc0;
        }
        
        .metric-card {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 20px;
            border-radius: 8px;
            margin: 10px 0;
        }
        
        .metric-value {
            font-size: 2.5em;
            font-weight: bold;
            margin: 10px 0;
        }
        
        .metric-label {
            font-size: 0.9em;
            opacity: 0.9;
        }
        
        .documentation-form {
            background: #f5f5f5;
            padding: 20px;
            border-radius: 8px;
            margin-top: 15px;
        }
        
        .form-group {
            margin-bottom: 15px;
        }
        
        .form-label {
            display: block;
            font-weight: 600;
            margin-bottom: 5px;
            color: #333;
        }
        
        .form-control {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 0.95em;
        }
        
        .checkbox-group {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        
        .checkbox-item {
            display: flex;
            align-items: center;
            gap: 8px;
            padding: 10px;
            background: white;
            border-radius: 4px;
            cursor: pointer;
        }
        
        .checkbox-item:hover {
            background: #e3f2fd;
        }
        
        .risk-indicator {
            display: inline-block;
            padding: 4px 12px;
            border-radius: 12px;
            font-size: 0.85em;
            font-weight: 600;
            margin-left: 10px;
        }
        
        .risk-high {
            background: #ffcdd2;
            color: #c62828;
        }
        
        .risk-medium {
            background: #fff9c4;
            color: #f57f17;
        }
        
        .risk-low {
            background: #c8e6c9;
            color: #2e7d32;
        }
        
        .timeline {
            position: relative;
            padding-left: 30px;
            margin: 20px 0;
        }
        
        .timeline::before {
            content: '';
            position: absolute;
            left: 10px;
            top: 0;
            bottom: 0;
            width: 2px;
            background: #667eea;
        }
        
        .timeline-item {
            position: relative;
            margin-bottom: 20px;
            padding: 15px;
            background: #f8f9fa;
            border-radius: 6px;
        }
        
        .timeline-item::before {
            content: '';
            position: absolute;
            left: -24px;
            top: 20px;
            width: 12px;
            height: 12px;
            border-radius: 50%;
            background: #667eea;
            border: 3px solid white;
        }
        
        .timeline-date {
            font-weight: 600;
            color: #667eea;
            font-size: 0.9em;
        }
        
        .timeline-content {
            margin-top: 5px;
            color: #555;
        }
        
        .dashboard-summary {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin: 20px 0;
        }
        
        .summary-card {
            background: white;
            padding: 20px;
            border-radius: 8px;
            border-left: 4px solid #667eea;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        
        .summary-number {
            font-size: 2em;
            font-weight: bold;
            color: #667eea;
        }
        
        .summary-label {
            color: #666;
            margin-top: 5px;
            font-size: 0.9em;
        }
        
        .education-material {
            background: #e1f5fe;
            padding: 15px;
            border-radius: 6px;
            margin-top: 15px;
            border-left: 4px solid #0288d1;
        }
        
        .status-badge {
            display: inline-block;
            padding: 4px 12px;
            border-radius: 12px;
            font-size: 0.85em;
            font-weight: 600;
        }
        
        .status-due {
            background: #fff3cd;
            color: #856404;
        }
        
        .status-overdue {
            background: #f8d7da;
            color: #721c24;
        }
        
        .status-complete {
            background: #d4edda;
            color: #155724;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🏥 Clinical Decision Support System - Pneumococcal Vaccination</h1>
        
        <!-- Example 1: CDS Input Data Collection -->
        <div class="dashboard-grid">
            <div class="card">
                <div class="card-title">📊 Step 1: CDS Inputs - Patient Data Collection</div>
                <div class="ehr-screen">
                    <div class="patient-banner">
                        <div class="patient-info">
                            <div class="info-item">
                                <span class="info-label">Patient Name</span>
                                <span class="info-value">Johnson, Margaret</span>
                            </div>
                            <div class="info-item">
                                <span class="info-label">DOB</span>
                                <span class="info-value">03/15/1958 (Age: 66)</span>
                            </div>
                            <div class="info-item">
                                <span class="info-label">MRN</span>
                                <span class="info-value">MRN-789456</span>
                            </div>
                            <div class="info-item">
                                <span class="info-label">Visit Type</span>
                                <span class="info-value">Annual Wellness</span>
                            </div>
                            <div class="info-item">
                                <span class="info-label">Provider</span>
                                <span class="info-value"> Abdul-Ghaniyu Alidu, RN</span>
                            </div>
                            <div class="info-item">
                                <span class="info-label">Visit Date</span>
                                <span class="info-value">02/07/2026</span>
                            </div>
                        </div>
                    </div>
                    
                    <h4 style="margin-top: 15px; color: #333;">Active Problem List</h4>
                    <div class="checkbox-group">
                        <div class="checkbox-item">
                            <input type="checkbox" checked disabled>
                            <span>Type 2 Diabetes Mellitus (E11.9) <span class="risk-indicator risk-high">HIGH RISK</span></span>
                        </div>
                        <div class="checkbox-item">
                            <input type="checkbox" checked disabled>
                            <span>Hypertension (I10) <span class="risk-indicator risk-medium">MEDIUM RISK</span></span>
                        </div>
                        <div class="checkbox-item">
                            <input type="checkbox" checked disabled>
                            <span>COPD (J44.9) <span class="risk-indicator risk-high">HIGH RISK</span></span>
                        </div>
                    </div>
                    
                    <h4 style="margin-top: 15px; color: #333;">Immunization History</h4>
                    <div class="timeline">
                        <div class="timeline-item">
                            <div class="timeline-date">09/12/2018</div>
                            <div class="timeline-content">
                                <strong>PPSV23</strong> (CVX: 33) - Administered at this clinic<br>
                                <small>Lot: PNV2018-4567 | NDC: 00006-4739-00</small>
                            </div>
                        </div>
                        <div class="timeline-item">
                            <div class="timeline-date">11/05/2025</div>
                            <div class="timeline-content">
                                <strong>Influenza Vaccine</strong> (CVX: 141) - Current season<br>
                                <small>Up to date</small>
                            </div>
                        </div>
                    </div>
                    
                    <div style="background: #fff3cd; padding: 10px; border-radius: 4px; margin-top: 15px;">
                        <strong>⚠️ System Note:</strong> Last PPSV23 >5 years ago. Patient qualifies for revaccination per CDC guidelines (age 65+, chronic conditions).
                    </div>
                </div>
            </div>
            
            <!-- Example 2: CDS Trigger Logic -->
            <div class="card">
                <div class="card-title">🔔 Step 2: CDS Trigger - Decision Logic</div>
                <div class="data-flow">
                    <div class="flow-step">
                        <strong>Age Check</strong><br>
                        66 years ✓
                    </div>
                    <span class="flow-arrow">→</span>
                    <div class="flow-step">
                        <strong>Risk Status</strong><br>
                        High Risk ✓
                    </div>
                    <span class="flow-arrow">→</span>
                    <div class="flow-step">
                        <strong>Last Vaccine</strong><br>
                        >5 years ✓
                    </div>
                </div>
                
                <div style="background: #e8f5e9; padding: 15px; border-radius: 8px; margin: 15px 0; border-left: 4px solid #4caf50;">
                    <h4 style="color: #2e7d32; margin-bottom: 10px;">✓ Trigger Criteria Met</h4>
                    <ul style="margin-left: 20px; color: #555; line-height: 1.8;">
                        <li><strong>Age:</strong> 66 years (≥65 qualifies)</li>
                        <li><strong>High-risk conditions:</strong> Diabetes, COPD</li>
                        <li><strong>Time since last PPSV23:</strong> 7 years, 5 months</li>
                        <li><strong>No documented contraindications</strong></li>
                        <li><strong>No recent refusal</strong> (last 12 months)</li>
                        <li><strong>Visit type:</strong> Preventive care (optimal timing)</li>
                    </ul>
                </div>
                
                <div class="metric-card">
                    <div class="metric-label">Recommendation Confidence Score</div>
                    <div class="metric-value">98%</div>
                    <div style="font-size: 0.9em;">Strong recommendation for PPSV23 administration today</div>
                </div>
                
                <div style="background: #f5f5f5; padding: 15px; border-radius: 8px; margin-top: 15px;">
                    <h4 style="color: #333; margin-bottom: 10px;">CDC Guideline Reference</h4>
                    <div style="font-size: 0.9em; color: #555; line-height: 1.6;">
                        <strong>ACIP Recommendation:</strong> Adults aged ≥65 years should receive one dose of PPSV23. 
                        Those who received PPSV23 before age 65 for a high-risk condition should receive another dose 
                        at age 65 or later (at least 5 years after prior dose).
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Example 3: CDS Intervention Alert -->
        <div class="dashboard-grid">
            <div class="card">
                <div class="card-title">💡 Step 3: CDS Intervention - Best Practice Alert</div>
                <div class="ehr-screen">
                    <div class="alert-box">
                        <div class="alert-header">
                            <span class="alert-icon">💉</span>
                            <span class="alert-title">PPSV23 Pneumococcal Vaccine DUE</span>
                            <span class="status-badge status-due">ACTION REQUIRED</span>
                        </div>
                        <div class="alert-message">
                            <strong>Patient qualifies for pneumococcal revaccination today.</strong><br><br>
                            
                            <strong>Qualifying criteria:</strong>
                            <ul style="margin: 10px 0 10px 20px; line-height: 1.6;">
                                <li>Age: 66 years (≥65)</li>
                                <li>High-risk conditions: Type 2 Diabetes, COPD</li>
                                <li>Last PPSV23: 09/12/2018 (7 years, 5 months ago)</li>
                            </ul>
                            
                            <strong>Recommended action:</strong> Administer PPSV23 (Pneumovax 23) 0.5mL IM today
                        </div>
                        
                        <div class="button-group">
                            <button class="btn btn-primary" onclick="showOrderSet()">
                                ✓ Order PPSV23 Vaccine
                            </button>
                            <button class="btn btn-secondary" onclick="showEducation()">
                                📄 View Patient Education
                            </button>
                            <button class="btn btn-warning" onclick="showDocumentation()">
                                📝 Document Reason Not Given
                            </button>
                            <button class="btn btn-danger" onclick="snoozeAlert()">
                                ⏰ Defer to Next Visit
                            </button>
                        </div>
                    </div>
                    
                    <div id="educationMaterial" class="education-material" style="display: none;">
                        <h4 style="color: #01579b; margin-bottom: 10px;">📋 Patient Education Material</h4>
                        <div style="font-size: 0.9em; line-height: 1.6; color: #555;">
                            <strong>What is PPSV23?</strong><br>
                            Pneumococcal polysaccharide vaccine protects against 23 types of bacteria that cause pneumococcal disease.<br><br>
                            
                            <strong>Why is this recommended?</strong><br>
                            Your age and health conditions put you at higher risk for serious pneumococcal infections, including pneumonia, meningitis, and bloodstream infections.<br><br>
                            
                            <strong>Common side effects:</strong> Mild soreness at injection site, low-grade fever (1-2 days)<br><br>
                            
                            <strong>Available in:</strong> English, Spanish, Chinese, Vietnamese
                        </div>
                        <button class="btn btn-secondary" style="margin-top: 10px;" onclick="printEducation()">
                            🖨️ Print for Patient
                        </button>
                    </div>
                </div>
            </div>
            
            <!-- Example 4: Action Pathway - Order Set -->
            <div class="card">
                <div class="card-title">✅ Step 4a: Action Pathway - Vaccine Order</div>
                <div class="ehr-screen">
                    <div style="background: #d4edda; padding: 15px; border-radius: 8px; border-left: 4px solid #28a745; margin-bottom: 15px;">
                        <h4 style="color: #155724; margin-bottom: 10px;">✓ One-Click Order Set Activated</h4>
                        <p style="color: #155724; margin: 0;">Pre-populated vaccine order with patient-specific parameters</p>
                    </div>
                    
                    <div class="documentation-form">
                        <h4 style="margin-bottom: 15px; color: #333;">Vaccine Order Details</h4>
                        
                        <div class="form-group">
                            <label class="form-label">Vaccine</label>
                            <input type="text" class="form-control" value="PPSV23 (Pneumovax 23)" readonly>
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label">CVX Code</label>
                            <input type="text" class="form-control" value="33 - Pneumococcal polysaccharide vaccine" readonly>
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label">CPT Code</label>
                            <input type="text" class="form-control" value="90732 - Pneumococcal vaccine, polysaccharide, 23-valent" readonly>
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label">Dose & Route</label>
                            <input type="text" class="form-control" value="0.5 mL, Intramuscular (IM)" readonly>
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label">Administration Site</label>
                            <select class="form-control">
                                <option>Left deltoid (preferred)</option>
                                <option>Right deltoid</option>
                                <option>Left vastus lateralis</option>
                                <option>Right vastus lateralis</option>
                            </select>
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label">VIS (Vaccine Information Statement) Date</label>
                            <input type="text" class="form-control" value="10/06/2024" readonly>
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label">Auto-Generated Nursing Task</label>
                            <textarea class="form-control" rows="3" readonly>✓ Administer PPSV23 0.5mL IM
✓ Provide VIS to patient and document
✓ Observe patient for 15 minutes post-vaccination
✓ Document lot number, expiration date, administration site</textarea>
                        </div>
                        
                        <button class="btn btn-primary" style="width: 100%; margin-top: 10px;">
                            📋 Sign and Submit Order
                        </button>
                    </div>
                    
                    <div style="background: #e3f2fd; padding: 15px; border-radius: 8px; margin-top: 15px;">
                        <strong>📊 Auto-Documentation:</strong> Order will populate After-Visit Summary, update immunization registry, and trigger quality measure numerator inclusion.
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Example 5: Alternative Pathway - Documented Refusal -->
        <div class="dashboard-grid">
            <div class="card">
                <div class="card-title">📝 Step 4b: Alternative Action - Document Reason Not Given</div>
                <div class="documentation-form">
                    <h4 style="margin-bottom: 15px; color: #333;">Structured Documentation Form</h4>
                    
                    <div class="form-group">
                        <label class="form-label">Category (Required) *</label>
                        <select class="form-control" id="reasonCategory" onchange="updateReasonOptions()">
                            <option value="">-- Select Category --</option>
                            <option value="medical">Medical Contraindication</option>
                            <option value="patient">Patient Refusal</option>
                            <option value="system">System/Organizational Barrier</option>
                            <option value="clinical">Clinical Decision to Defer</option>
                        </select>
                    </div>
                    
                    <div class="form-group" id="specificReason" style="display: none;">
                        <label class="form-label">Specific Reason (Required) *</label>
                        <select class="form-control" id="reasonDetail">
                            <!-- Options populated dynamically -->
                        </select>
                    </div>
                    
                    <div class="form-group" id="codingSection" style="display: none;">
                        <label class="form-label">Auto-Applied Code</label>
                        <input type="text" class="form-control" id="autoCode" readonly>
                    </div>
                    
                    <div class="form-group" id="educationAttestation" style="display: none;">
                        <div style="background: #fff3cd; padding: 15px; border-radius: 6px; margin-bottom: 10px;">
                            <strong>⚠️ Required for Patient Refusal:</strong>
                        </div>
                        <div class="checkbox-item">
                            <input type="checkbox" id="eduProvided">
                            <label for="eduProvided">✓ I have provided vaccine education and discussed risks/benefits with patient</label>
                        </div>
                        <div class="checkbox-item">
                            <input type="checkbox" id="patientAck">
                            <label for="patientAck">✓ Patient acknowledges understanding and confirms refusal decision</label>
                        </div>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Additional Notes (Optional)</label>
                        <textarea class="form-control" rows="3" placeholder="Any additional context or follow-up plans..."></textarea>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Follow-up Action</label>
                        <select class="form-control">
                            <option>Reassess at next annual visit</option>
                            <option>Reassess in 6 months</option>
                            <option>Reassess in 3 months</option>
                            <option>Patient to contact when ready</option>
                            <option>No further follow-up needed (permanent contraindication)</option>
                        </select>
                    </div>
                    
                    <button class="btn btn-primary" style="width: 100%; margin-top: 10px;">
                        💾 Save Documentation
                    </button>
                </div>
                
                <div style="background: #f5f5f5; padding: 15px; border-radius: 8px; margin-top: 15px;">
                    <h4 style="color: #333; margin-bottom: 10px;">📊 Quality Measure Impact</h4>
                    <div style="font-size: 0.9em; color: #555; line-height: 1.8;">
                        <strong>Medical Contraindication:</strong> Excluded from denominator<br>
                        <strong>Patient Refusal (properly documented):</strong> Excluded from denominator<br>
                        <strong>System Barrier:</strong> Excluded from denominator, flagged for operations review<br>
                        <strong>Clinical Deferral:</strong> Remains in denominator, reassessed at follow-up
                    </div>
                </div>
            </div>
            
            <!-- Example 6: Provider Dashboard -->
            <div class="card">
                <div class="card-title">📈 Quality Dashboard - Real-Time Feedback</div>
                
                <div class="dashboard-summary">
                    <div class="summary-card">
                        <div class="summary-number">87%</div>
                        <div class="summary-label">Vaccination Rate<br>(Your Panel)</div>
                    </div>
                    <div class="summary-card">
                        <div class="summary-number">91%</div>
                        <div class="summary-label">Clinic Average</div>
                    </div>
                    <div class="summary-card">
                        <div class="summary-number">85%</div>
                        <div class="summary-label">National Benchmark</div>
                    </div>
                    <div class="summary-card">
                        <div class="summary-number">42</div>
                        <div class="summary-label">Patients Due Now</div>
                    </div>
                </div>
                
                <div style="background: #fff; padding: 20px; border-radius: 8px; border: 2px solid #e0e0e0; margin-top: 15px;">
                    <h4 style="color: #333; margin-bottom: 15px;">This Month's Activity (February 2026)</h4>
                    
                    <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-bottom: 15px;">
                        <div style="background: #e8f5e9; padding: 15px; border-radius: 6px;">
                            <div style="font-size: 2em; color: #2e7d32; font-weight: bold;">23</div>
                            <div style="color: #555; font-size: 0.9em;">Vaccines Administered</div>
                        </div>
                        <div style="background: #fff3cd; padding: 15px; border-radius: 6px;">
                            <div style="font-size: 2em; color: #856404; font-weight: bold;">8</div>
                            <div style="color: #555; font-size: 0.9em;">Patient Refusals</div>
                        </div>
                        <div style="background: #ffebee; padding: 15px; border-radius: 6px;">
                            <div style="font-size: 2em; color: #c62828; font-weight: bold;">2</div>
                            <div style="color: #555; font-size: 0.9em;">Medical Contraindications</div>
                        </div>
                        <div style="background: #e3f2fd; padding: 15px; border-radius: 6px;">
                            <div style="font-size: 2em; color: #1565c0; font-weight: bold;">1</div>
                            <div style="color: #555; font-size: 0.9em;">System Barriers</div>
                        </div>
                    </div>
                    
                    <div style="background: #f5f5f5; padding: 15px; border-radius: 6px;">
                        <strong>📊 Breakdown of Exclusions:</strong>
                        <ul style="margin: 10px 0 0 20px; line-height: 1.8; color: #555;">
                            <li><strong>Patient Refusals (8):</strong> All properly documented with education attestation ✓</li>
                            <li><strong>Medical Contraindications (2):</strong>
                                <ul style="margin-left: 20px;">
                                    <li>1 - Previous severe allergic reaction (SNOMED: 293104008)</li>
                                    <li>1 - Receiving immunosuppressive therapy</li>
                                </ul>
                            </li>
                            <li><strong>System Barrier (1):</strong> Vaccine stockout 02/01/2026 (escalated to operations)</li>
                        </ul>
                    </div>
                </div>
                
                <div class="metric-card" style="margin-top: 15px;">
                    <div style="font-size: 1.1em; margin-bottom: 10px;">🎯 Performance Trend</div>
                    <div style="font-size: 0.95em;">
                        Your vaccination rate has improved by <strong>5%</strong> over the last quarter.
                        You're in the <strong>top 25%</strong> of providers in the system.
                    </div>
                </div>
            </div>
        </div>
        
    </div>
    
    <script>
        function showOrderSet() {
            alert('✓ One-click order set activated!\n\nThe vaccine order form has been pre-populated with:\n- PPSV23 vaccine details\n- Appropriate CVX and CPT codes\n- Patient-specific dose and route\n- Auto-generated nursing tasks\n- Quality measure documentation');
        }
        
        function showEducation() {
            const edu = document.getElementById('educationMaterial');
            edu.style.display = edu.style.display === 'none' ? 'block' : 'none';
        }
        
        function showDocumentation() {
            alert('📝 Structured documentation form opened.\n\nThis ensures proper coding for quality measure exclusions and creates an auditable trail.');
        }
        
        function snoozeAlert() {
            if (confirm('Defer this alert to the patient\'s next visit?\n\nNote: Patient will remain in quality measure denominator until vaccine is administered or exclusion is documented.')) {
                alert('✓ Alert deferred.\n\nReminder will appear at next encounter.');
            }
        }
        
        function printEducation() {
            alert('🖨️ Printing patient education materials in preferred language.\n\nDocument will be added to After-Visit Summary automatically.');
        }
        
        function updateReasonOptions() {
            const category = document.getElementById('reasonCategory').value;
            const detailSelect = document.getElementById('reasonDetail');
            const specificReason = document.getElementById('specificReason');
            const codingSection = document.getElementById('codingSection');
            const autoCode = document.getElementById('autoCode');
            const eduAttest = document.getElementById('educationAttestation');
            
            specificReason.style.display = category ? 'block' : 'none';
            codingSection.style.display = category ? 'block' : 'none';
            eduAttest.style.display = category === 'patient' ? 'block' : 'none';
            
            detailSelect.innerHTML = '';
            
            const options = {
                medical: [
                    {text: 'Previous severe allergic reaction', code: 'SNOMED CT: 293104008'},
                    {text: 'Current acute moderate/severe illness', code: 'ICD-10: Z28.04'},
                    {text: 'Immunosuppressive therapy', code: 'SNOMED CT: 183932001'},
                    {text: 'Pregnancy (if applicable)', code: 'ICD-10: Z28.04'}
                ],
                patient: [
                    {text: 'Patient declined after education', code: 'ICD-10: Z28.21 / SNOMED: 591000119102'},
                    {text: 'Patient wants to discuss with family first', code: 'ICD-10: Z28.21'},
                    {text: 'Religious/philosophical objection', code: 'ICD-10: Z28.21'},
                    {text: 'Concerned about side effects', code: 'ICD-10: Z28.21'}
                ],
                system: [
                    {text: 'Vaccine not available/stockout', code: 'ICD-10: Z28.83'},
                    {text: 'Insurance authorization pending', code: 'ICD-10: Z28.89'},
                    {text: 'Equipment malfunction (refrigeration)', code: 'ICD-10: Z28.89'},
                    {text: 'Staffing shortage', code: 'ICD-10: Z28.89'}
                ],
                clinical: [
                    {text: 'Defer to next scheduled preventive visit', code: 'Provider discretion'},
                    {text: 'Patient too ill today (acute visit)', code: 'Provider discretion'},
                    {text: 'Multiple vaccines already given today', code: 'Provider discretion'}
                ]
            };
            
            if (options[category]) {
                options[category].forEach(opt => {
                    const option = document.createElement('option');
                    option.value = opt.code;
                    option.textContent = opt.text;
                    detailSelect.appendChild(option);
                });
                autoCode.value = options[category][0].code;
                
                detailSelect.onchange = function() {
                    autoCode.value = this.value;
                };
            }
        }
    </script>
</body>
</html>
