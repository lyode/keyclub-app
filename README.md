<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>1+1.5 KeyClub Members Management</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest/dist/umd/lucide.js"></script>
    <style>
        .modal-overlay {
            display: none;
            position: fixed;
            inset: 0;
            background: rgba(0, 0, 0, 0.5);
            z-index: 50;
        }
        .modal-overlay.active {
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 1rem;
        }
        .tab-button {
            transition: all 0.2s;
        }
        .tab-button.active {
            background-color: rgb(243, 232, 255);
            color: rgb(147, 51, 234);
        }
        .tab-button:not(.active):hover {
            background-color: rgb(243, 244, 246);
        }
    </style>
</head>
<body class="min-h-screen bg-gray-50">
    <!-- Header -->
    <div class="bg-gradient-to-r from-purple-600 to-pink-600 text-white p-4 shadow-lg">
        <h1 class="text-2xl font-bold">1+1.5 KeyClub Members</h1>
        <p class="text-sm opacity-90">Member Management System</p>
    </div>

    <!-- Navigation -->
    <div class="bg-white shadow-sm">
        <div class="flex space-x-6 p-4">
            <button onclick="showTab('dashboard')" class="tab-button active flex items-center space-x-2 px-4 py-2 rounded-lg" id="dashboard-tab">
                <i data-lucide="trending-up"></i>
                <span>Dashboard</span>
            </button>
            <button onclick="showTab('members')" class="tab-button flex items-center space-x-2 px-4 py-2 rounded-lg text-gray-600" id="members-tab">
                <i data-lucide="users"></i>
                <span>Members</span>
            </button>
            <button onclick="showTab('analysis')" class="tab-button flex items-center space-x-2 px-4 py-2 rounded-lg text-gray-600" id="analysis-tab">
                <i data-lucide="bar-chart-3"></i>
                <span>Analysis</span>
            </button>
            <button onclick="showTab('alerts')" class="tab-button flex items-center space-x-2 px-4 py-2 rounded-lg text-gray-600" id="alerts-tab">
                <i data-lucide="bell"></i>
                <span>Alerts</span>
            </button>
        </div>
    </div>

    <!-- Content -->
    <div class="p-6">
        <!-- Dashboard Tab -->
        <div id="dashboard-content" class="tab-content">
            <h2 class="text-xl font-bold mb-6">Daily Summary</h2>
            
            <div class="grid grid-cols-1 md:grid-cols-4 gap-6 mb-8">
                <div class="bg-white p-6 rounded-lg shadow-sm border-l-4 border-green-500">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-sm text-gray-600">New Members Today</p>
                            <p class="text-2xl font-bold text-green-600" id="new-members-count">3</p>
                        </div>
                        <i data-lucide="user-plus" class="text-green-500"></i>
                    </div>
                </div>
                
                <div class="bg-white p-6 rounded-lg shadow-sm border-l-4 border-blue-500">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-sm text-gray-600">Total Collection</p>
                            <p class="text-2xl font-bold text-blue-600" id="total-collection">RM450</p>
                        </div>
                        <i data-lucide="dollar-sign" class="text-blue-500"></i>
                    </div>
                </div>
                
                <div class="bg-white p-6 rounded-lg shadow-sm border-l-4 border-orange-500">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-sm text-gray-600">Expiring Today</p>
                            <p class="text-2xl font-bold text-orange-600" id="expiring-today">1</p>
                        </div>
                        <i data-lucide="clock" class="text-orange-500"></i>
                    </div>
                </div>
                
                <div class="bg-white p-6 rounded-lg shadow-sm border-l-4 border-purple-500">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-sm text-gray-600">Active Members</p>
                            <p class="text-2xl font-bold text-purple-600" id="active-members">2</p>
                        </div>
                        <i data-lucide="users" class="text-purple-500"></i>
                    </div>
                </div>
            </div>

            <div class="bg-white p-6 rounded-lg shadow-sm">
                <h3 class="text-lg font-semibold mb-4">Member Benefits</h3>
                <div class="space-y-3">
                    <div class="flex items-center space-x-3">
                        <i data-lucide="gift" class="text-purple-500"></i>
                        <span>RM1 only for 1 plate fried rice</span>
                    </div>
                    <div class="flex items-center space-x-3">
                        <i data-lucide="user-plus" class="text-green-500"></i>
                        <span>Introduce 3 members = Prolong membership 1 week</span>
                    </div>
                </div>
            </div>
        </div>

        <!-- Members Tab -->
        <div id="members-content" class="tab-content hidden">
            <div class="flex justify-between items-center mb-6">
                <h2 class="text-xl font-bold">Members Management</h2>
                <button onclick="showAddMemberModal()" class="bg-purple-600 text-white px-4 py-2 rounded-lg flex items-center space-x-2 hover:bg-purple-700 transition-colors">
                    <i data-lucide="plus"></i>
                    <span>Add Member</span>
                </button>
            </div>

            <div class="bg-white rounded-lg shadow-sm overflow-hidden">
                <table class="w-full">
                    <thead class="bg-gray-50">
                        <tr>
                            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Member</th>
                            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Branch</th>
                            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Duration</th>
                            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Status</th>
                            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Referrals</th>
                            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Actions</th>
                        </tr>
                    </thead>
                    <tbody id="members-table" class="divide-y divide-gray-200">
                        <!-- Members will be populated by JavaScript -->
                    </tbody>
                </table>
            </div>
        </div>

        <!-- Analysis Tab -->
        <div id="analysis-content" class="tab-content hidden">
            <h2 class="text-xl font-bold mb-6">Member Analysis</h2>
            
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-6">
                <div class="bg-white p-6 rounded-lg shadow-sm border-l-4 border-blue-500">
                    <h3 class="text-lg font-semibold text-gray-900 mb-2">Total Members</h3>
                    <p class="text-3xl font-bold text-blue-600" id="analysis-total-members">4</p>
                    <p class="text-sm text-gray-500 mt-1" id="analysis-active-members">2 Active</p>
                </div>
                
                <div class="bg-white p-6 rounded-lg shadow-sm border-l-4 border-green-500">
                    <h3 class="text-lg font-semibold text-gray-900 mb-2">Total Revenue</h3>
                    <p class="text-3xl font-bold text-green-600" id="analysis-total-revenue">RM440</p>
                    <p class="text-sm text-gray-500 mt-1">All time earnings</p>
                </div>
                
                <div class="bg-white p-6 rounded-lg shadow-sm border-l-4 border-purple-500">
                    <h3 class="text-lg font-semibold text-gray-900 mb-2">Avg. Revenue</h3>
                    <p class="text-3xl font-bold text-purple-600" id="analysis-avg-revenue">RM110</p>
                    <p class="text-sm text-gray-500 mt-1">Per member</p>
                </div>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                <div class="bg-white p-6 rounded-lg shadow-sm">
                    <h3 class="text-lg font-semibold mb-4">Members by Branch</h3>
                    <div id="branch-analysis" class="space-y-3">
                        <!-- Branch analysis will be populated by JavaScript -->
                    </div>
                </div>
                
                <div class="bg-white p-6 rounded-lg shadow-sm">
                    <h3 class="text-lg font-semibold mb-4">Revenue by Branch</h3>
                    <div id="revenue-analysis" class="space-y-3">
                        <!-- Revenue analysis will be populated by JavaScript -->
                    </div>
                </div>
            </div>
        </div>

        <!-- Alerts Tab -->
        <div id="alerts-content" class="tab-content hidden">
            <h2 class="text-xl font-bold mb-6">Membership Alerts</h2>
            <div id="alerts-list" class="space-y-4">
                <!-- Alerts will be populated by JavaScript -->
            </div>
        </div>
    </div>

    <!-- Add Member Modal -->
    <div id="add-member-modal" class="modal-overlay">
        <div class="bg-white rounded-lg p-6 w-full max-w-md">
            <h3 class="text-lg font-semibold mb-4">Add New Member</h3>
            
            <div class="space-y-4">
                <input type="text" id="member-name" placeholder="Full Name" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent">
                <input type="email" id="member-email" placeholder="Email" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent">
                <input type="tel" id="member-phone" placeholder="Phone Number" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent">
                <input type="text" id="member-branch" placeholder="Branch Name" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent">
                <select id="member-duration" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent">
                    <option value="1 month">1 month - RM30</option>
                    <option value="3 months">3 months - RM80</option>
                    <option value="6 months">6 months - RM150</option>
                    <option value="1 year">1 year - RM280</option>
                </select>
            </div>
            
            <div class="flex space-x-3 mt-6">
                <button onclick="hideAddMemberModal()" class="flex-1 py-2 px-4 border border-gray-300 rounded-lg hover:bg-gray-50 transition-colors">Cancel</button>
                <button onclick="addMember()" class="flex-1 py-2 px-4 bg-purple-600 text-white rounded-lg hover:bg-purple-700 transition-colors">Add Member</button>
            </div>
        </div>
    </div>

    <!-- Member Card Modal -->
    <div id="member-card-modal" class="modal-overlay">
        <div class="bg-white rounded-lg p-6 max-w-sm w-full">
            <div class="flex justify-between items-center mb-4">
                <h3 class="text-lg font-semibold">Member Card</h3>
                <button onclick="hideMemberCardModal()" class="text-gray-500 hover:text-gray-700">✕</button>
            </div>
            
            <div id="member-card-content">
                <!-- Member card will be populated by JavaScript -->
            </div>
            
            <div class="mt-4 space-y-2">
                <button class="w-full py-2 px-4 bg-purple-600 text-white rounded-lg hover:bg-purple-700 transition-colors">Download Card</button>
                <button class="w-full py-2 px-4 border border-gray-300 rounded-lg hover:bg-gray-50 transition-colors">Send via WhatsApp</button>
            </div>
        </div>
    </div>

    <script>
        // Sample data
        let members = [
            {
                id: 1,
                name: 'John Tan',
                email: 'john@email.com',
                phone: '+60123456789',
                branch: 'Petaling Jaya',
                joinDate: '2025-06-01',
                expiryDate: '2025-12-01',
                duration: '6 months',
                status: 'Active',
                referrals: 2,
                qrCode: 'KC001'
            },
            {
                id: 2,
                name: 'Sarah Lee',
                email: 'sarah@email.com',
                phone: '+60198765432',
                branch: 'Shah Alam',
                joinDate: '2025-05-15',
                expiryDate: '2025-06-15',
                duration: '1 month',
                status: 'Expiring Soon',
                referrals: 1,
                qrCode: 'KC002'
            },
            {
                id: 3,
                name: 'Ahmad Rahman',
                email: 'ahmad@email.com',
                phone: '+60187654321',
                branch: 'Petaling Jaya',
                joinDate: '2025-06-10',
                expiryDate: '2025-09-10',
                duration: '3 months',
                status: 'Active',
                referrals: 0,
                qrCode: 'KC003'
            },
            {
                id: 4,
                name: 'Lisa Wong',
                email: 'lisa@email.com',
                phone: '+60176543210',
                branch: 'Shah Alam',
                joinDate: '2025-04-01',
                expiryDate: '2025-06-01',
                duration: '1 month',
                status: 'Inactive',
                referrals: 3,
                qrCode: 'KC004'
            }
        ];

        const durationPrices = {
            '1 month': 30,
            '3 months': 80,
            '6 months': 150,
            '1 year': 280
        };

        // Initialize
        document.addEventListener('DOMContentLoaded', function() {
            lucide.createIcons();
            updateDashboard();
            updateMembersTable();
            updateAnalysis();
            updateAlerts();
        });

        // Tab switching
        function showTab(tabName) {
            // Hide all content
            document.querySelectorAll('.tab-content').forEach(content => {
                content.classList.add('hidden');
            });
            
            // Remove active class from all tabs
            document.querySelectorAll('.tab-button').forEach(tab => {
                tab.classList.remove('active');
                tab.classList.add('text-gray-600');
            });
            
            // Show selected content
            document.getElementById(tabName + '-content').classList.remove('hidden');
            
            // Add active class to selected tab
            const activeTab = document.getElementById(tabName + '-tab');
            activeTab.classList.add('active');
            activeTab.classList.remove('text-gray-600');
        }

        // Update dashboard stats
        function updateDashboard() {
            const activeMembers = members.filter(m => m.status === 'Active').length;
            const totalRevenue = members.reduce((sum, member) => {
                return sum + (durationPrices[member.duration] || 0);
            }, 0);
            
            document.getElementById('active-members').textContent = activeMembers;
            document.getElementById('total-collection').textContent = `RM${totalRevenue}`;
        }

        // Update members table
        function updateMembersTable() {
            const tbody = document.getElementById('members-table');
            tbody.innerHTML = '';
            
            members.forEach(member => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td class="px-6 py-4">
                        <div>
                            <div class="font-medium text-gray-900">${member.name}</div>
                            <div class="text-sm text-gray-500">${member.email}</div>
                        </div>
                    </td>
                    <td class="px-6 py-4">
                        <div class="flex items-center space-x-1">
                            <i data-lucide="map-pin" class="w-4 h-4 text-gray-400"></i>
                            <span class="text-sm text-gray-900">${member.branch}</span>
                        </div>
                    </td>
                    <td class="px-6 py-4 text-sm text-gray-900">${member.duration}</td>
                    <td class="px-6 py-4">
                        <span class="px-2 py-1 text-xs rounded-full ${getStatusClass(member.status)}">
                            ${member.status}
                        </span>
                    </td>
                    <td class="px-6 py-4 text-sm text-gray-900">
                        ${member.referrals}/3
                        ${member.referrals >= 3 ? '<span class="ml-2 text-xs bg-green-100 text-green-800 px-2 py-1 rounded">+1 week earned</span>' : ''}
                    </td>
                    <td class="px-6 py-4">
                        <div class="flex space-x-2">
                            <button onclick="showMemberCard(${member.id})" class="text-purple-600 hover:text-purple-800 flex items-center space-x-1">
                                <i data-lucide="qr-code" class="w-4 h-4"></i>
                                <span class="text-sm">Card</span>
                            </button>
                            <button onclick="toggleMemberStatus(${member.id})" class="text-orange-600 hover:text-orange-800 flex items-center space-x-1">
                                <i data-lucide="user-x" class="w-4 h-4"></i>
                                <span class="text-sm">${member.status === 'Active' ? 'Deactivate' : 'Activate'}</span>
                            </button>
                            <button onclick="deleteMember(${member.id})" class="text-red-600 hover:text-red-800 flex items-center space-x-1">
                                <i data-lucide="trash-2" class="w-4 h-4"></i>
                                <span class="text-sm">Delete</span>
                            </button>
                        </div>
                    </td>
                `;
                tbody.appendChild(row);
            });
            
            // Re-create icons after updating DOM
            lucide.createIcons();
        }

        function getStatusClass(status) {
            switch(status) {
                case 'Active': return 'bg-green-100 text-green-800';
                case 'Expiring Soon': return 'bg-orange-100 text-orange-800';
                case 'Inactive': return 'bg-red-100 text-red-800';
                default: return 'bg-gray-100 text-gray-800';
            }
        }

        // Update analysis
        function updateAnalysis() {
            const totalMembers = members.length;
            const activeMembers = members.filter(m => m.status === 'Active').length;
            const totalRevenue = members.reduce((sum, member) => {
                return sum + (durationPrices[member.duration] || 0);
            }, 0);
            
            document.getElementById('analysis-total-members').textContent = totalMembers;
            document.getElementById('analysis-active-members').textContent = `${activeMembers} Active`;
            document.getElementById('analysis-total-revenue').textContent = `RM${totalRevenue}`;
            document.getElementById('analysis-avg-revenue').textContent = `RM${Math.round(totalRevenue / totalMembers)}`;
            
            // Branch analysis
            const branchStats = {};
            const branchRevenue = {};
            members.forEach(member => {
                branchStats[member.branch] = (branchStats[member.branch] || 0) + 1;
                branchRevenue[member.branch] = (branchRevenue[member.branch] || 0) + (durationPrices[member.duration] || 0);
            });
            
            // Update branch analysis
            const branchAnalysisDiv = document.getElementById('branch-analysis');
            branchAnalysisDiv.innerHTML = '';
            Object.entries(branchStats).forEach(([branch, count]) => {
                const percentage = Math.round((count / totalMembers) * 100);
                branchAnalysisDiv.innerHTML += `
                    <div class="flex justify-between items-center">
                        <div class="flex items-center space-x-2">
                            <i data-lucide="map-pin" class="w-4 h-4 text-gray-400"></i>
                            <span class="font-medium">${branch}</span>
                        </div>
                        <div class="flex items-center space-x-3">
                            <span class="text-sm text-gray-600">${count} members</span>
                            <div class="w-20 bg-gray-200 rounded-full h-2">
                                <div class="bg-purple-600 h-2 rounded-full" style="width: ${percentage}%"></div>
                            </div>
                            <span class="text-sm font-semibold text-purple-600">${percentage}%</span>
                        </div>
                    </div>
                `;
            });
            
            // Update revenue analysis
            const revenueAnalysisDiv = document.getElementById('revenue-analysis');
            revenueAnalysisDiv.innerHTML = '';
            Object.entries(branchRevenue).forEach(([branch, revenue]) => {
                const percentage = Math.round((revenue / totalRevenue) * 100);
                revenueAnalysisDiv.innerHTML += `
                    <div class="flex justify-between items-center">
                        <div class="flex items-center space-x-2">
                            <i data-lucide="dollar-sign" class="w-4 h-4 text-gray-400"></i>
                            <span class="font-medium">${branch}</span>
                        </div>
                        <div class="flex items-center space-x-3">
                            <span class="text-sm text-gray-600">RM${revenue}</span>
                            <div class="w-20 bg-gray-200 rounded-full h-2">
                                <div class="bg-green-600 h-2 rounded-full" style="width: ${percentage}%"></div>
                            </div>
                            <span class="text-sm font-semibold text-green-600">${percentage}%</span>
                        </div>
                    </div>
                `;
            });
            
            // Re-create icons
            lucide.createIcons();
        }

        // Update alerts
        function updateAlerts() {
            const alertsList = document.getElementById('alerts-list');
            alertsList.innerHTML = '';
            
            const today = new Date();
            const expiringMembers = members.filter(member => {
                const expiryDate = new Date(member.expiryDate);
                const diffTime = expiryDate - today;
                const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
                return diffDays <= 7 && diffDays >= 0;
            });
            
            expiringMembers.forEach(member => {
                const expiryDate = new Date(member.expiryDate);
                const diffTime = expiryDate - today;
                const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
                
                alertsList.innerHTML += `
                    <div class="bg-white p-4 rounded-lg shadow-sm border-l-4 border-orange-500">
                        <div class="flex justify-between items-center">
                            <div>
                                <h3 class="font-semibold text-gray-900">${member.name}</h3>
                                <p class="text-sm text-gray-600">Membership expires in ${diffDays} days</p>
                            </div>
                            <div class="text-right">
                                <p class="text-sm text-gray-500">Expires: ${expiryDate.toLocaleDateString()}</p>
                                <button class="text-purple-600 text-sm hover:underline">Send Reminder</button>
                            </div>
                        </div>
                    </div>
                `;
            });
            
            if (expiringMembers.length === 0) {
                alertsList.innerHTML = '<p class="text-gray-500 text-center py-8">No expiring memberships in the next 7 days.</p>';
            }
        }

        // Modal functions
        function showAddMemberModal() {
            document.getElementById('add-member-modal').classList.add('active');
        }

        function hideAddMemberModal() {
            document.getElementById('add-member-modal').classList.remove('active');
            // Clear form
            document.getElementById('member-name').value = '';
            document.getElementById('member-email').value = '';
            document.getElementById('member-phone').value = '';
            document.getElementById('member-branch').value = '';
            document.getElementById('member-duration').value = '1 month';
        }

        function addMember() {
            const name = document.getElementById('member-name').value;
            const email = document.getElementById('member-email').value;
            const phone = document.getElementById('member-phone').value;
            const branch = document.getElementById('member-branch').value;
            const duration = document.getElementById('member-duration').value;
            
            if (name && email && phone && branch) {
                const joinDate = new Date();
                const expiryDate = new Date();
                
                switch(duration) {
                    case '1 month': expiryDate.setMonth(expiryDate.getMonth() + 1); break;
                    case '3 months': expiryDate.setMonth(expiryDate.getMonth() + 3); break;
                    case '6 months': expiryDate.setMonth(expiryDate.getMonth() + 6); break;
                    case '1 year': expiryDate.setFullYear(expiryDate.getFullYear() + 1); break;
                }
                
                const newMember = {
                    id: members.length + 1,
                    name,
                    email,
                    phone,
                    branch,
                    duration,
                    joinDate: joinDate.toISOString().split('T')[0],
                    expiryDate: expiryDate.toISOString().split('T')[0],
                    status: 'Active',
                    referrals: 0,
                    qrCode: `KC${String(members.length + 1).padStart(3, '0')}`
                };
                
                members.push(newMember);
                updateDashboard();
                updateMembersTable();
                updateAnalysis();
                updateAlerts();
                hideAddMemberModal();
            }
        }

        function showMemberCard(memberId) {
            const member = members.find(m => m.id === memberId);
            if (member) {
                const cardContent = document.getElementById('member-card-content');
                cardContent.innerHTML = `
                    <div class="bg-gradient-to-br from-purple-600 via-pink-500 to-orange-400 p-6 rounded-2xl text-white shadow-2xl">
                        <div class="flex justify-between items-start mb-4">
                            <div>
                                <h3 class="text-xl font-bold">1+1.5 KeyClub</h3>
                                <p class="text-sm opacity-90">Premium Member</p>
                            </div>
                            <div class="bg-white/20 p-2 rounded-lg">
                                <img src="https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=${member.qrCode}" alt="QR Code" class="w-12 h-12">
                            </div>
                        </div>
                        
                        <div class="mb-4
