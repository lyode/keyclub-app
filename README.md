import React, { useState, useEffect } from 'react';

const KeyClubApp = () => {
  const [activeTab, setActiveTab] = useState('dashboard');
  const [members, setMembers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [showAddMember, setShowAddMember] = useState(false);
  const [showMemberCard, setShowMemberCard] = useState(null);
  
  const [newMember, setNewMember] = useState({
    name: '',
    email: '',
    phone: '',
    branch: '',
    duration: '1 month'
  });

  const durationOptions = [
    { value: '1 month', price: 30 },
    { value: '3 months', price: 80 },
    { value: '6 months', price: 150 },
    { value: '1 year', price: 280 }
  ];

  useEffect(() => {
    // Simulate loading members
    setTimeout(() => {
      setMembers([
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
        }
      ]);
      setLoading(false);
    }, 1000);
  }, []);

  const addMember = () => {
    if (newMember.name && newMember.email && newMember.phone && newMember.branch) {
      const joinDate = new Date();
      const expiryDate = new Date();
      
      switch(newMember.duration) {
        case '1 month': expiryDate.setMonth(expiryDate.getMonth() + 1); break;
        case '3 months': expiryDate.setMonth(expiryDate.getMonth() + 3); break;
        case '6 months': expiryDate.setMonth(expiryDate.getMonth() + 6); break;
        case '1 year': expiryDate.setFullYear(expiryDate.getFullYear() + 1); break;
      }

      const memberData = {
        id: members.length + 1,
        ...newMember,
        joinDate: joinDate.toISOString().split('T')[0],
        expiryDate: expiryDate.toISOString().split('T')[0],
        status: 'Active',
        referrals: 0,
        qrCode: `KC${String(members.length + 1).padStart(3, '0')}`
      };

      setMembers([...members, memberData]);
      setNewMember({ name: '', email: '', phone: '', branch: '', duration: '1 month' });
      setShowAddMember(false);
    }
  };

  const generateQRCode = (qrCode) => {
    return `https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=${encodeURIComponent(qrCode)}`;
  };

  const totalMembers = members.length;
  const activeMembers = members.filter(m => m.status === 'Active').length;
  const totalRevenue = members.reduce((sum, member) => {
    const price = durationOptions.find(d => d.value === member.duration)?.price || 0;
    return sum + price;
  }, 0);

  if (loading) {
    return (
      <div className="min-h-screen bg-gray-50 flex items-center justify-center">
        <div className="text-center">
          <div className="animate-spin rounded-full h-12 w-12 border-b-4 border-purple-500 mx-auto mb-4"></div>
          <h2 className="text-xl font-semibold text-gray-700 mb-2">Loading KeyClub System...</h2>
          <p className="text-gray-500">Initializing member management</p>
        </div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-gray-50">
      <div className="bg-gradient-to-r from-purple-600 to-pink-600 text-white p-4 shadow-lg">
        <div className="flex justify-between items-center">
          <div>
            <h1 className="text-2xl font-bold">1+1.5 KeyClub Members</h1>
            <p className="text-sm opacity-90">Professional Member Management System - LIVE!</p>
          </div>
        </div>
      </div>

      <div className="bg-white shadow-sm">
        <div className="flex space-x-6 p-4">
          {[
            { id: 'dashboard', label: 'Dashboard' },
            { id: 'members', label: 'Members' },
            { id: 'analytics', label: 'Analytics' }
          ].map(tab => (
            <button
              key={tab.id}
              onClick={() => setActiveTab(tab.id)}
              className={`flex items-center space-x-2 px-4 py-2 rounded-lg transition-colors ${
                activeTab === tab.id 
                  ? 'bg-purple-100 text-purple-600' 
                  : 'text-gray-600 hover:bg-gray-100'
              }`}
            >
              {tab.label}
            </button>
          ))}
        </div>
      </div>

      <div className="p-6">
        {activeTab === 'dashboard' && (
          <div>
            <h2 className="text-xl font-bold mb-6">KeyClub Dashboard</h2>
            
            <div className="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
              <div className="bg-white p-6 rounded-lg shadow-sm border-l-4 border-green-500">
                <div>
                  <p className="text-sm text-gray-600">Total Members</p>
                  <p className="text-2xl font-bold text-green-600">{totalMembers}</p>
                  <p className="text-xs text-gray-500 mt-1">{activeMembers} active</p>
                </div>
              </div>
              
              <div className="bg-white p-6 rounded-lg shadow-sm border-l-4 border-blue-500">
                <div>
                  <p className="text-sm text-gray-600">Total Revenue</p>
                  <p className="text-2xl font-bold text-blue-600">RM{totalRevenue}</p>
                  <p className="text-xs text-gray-500 mt-1">All time earnings</p>
                </div>
              </div>
              
              <div className="bg-white p-6 rounded-lg shadow-sm border-l-4 border-purple-500">
                <div>
                  <p className="text-sm text-gray-600">Health Score</p>
                  <p className="text-2xl font-bold text-purple-600">{totalMembers > 0 ? Math.round((activeMembers / totalMembers) * 100) : 0}%</p>
                  <p className="text-xs text-gray-500 mt-1">Member retention</p>
                </div>
              </div>
            </div>

            <div className="bg-white p-6 rounded-lg shadow-sm">
              <h3 className="text-lg font-semibold mb-4">🎉 Your KeyClub System is LIVE!</h3>
              <div className="mt-6 p-4 bg-gradient-to-r from-green-50 to-blue-50 rounded-lg border border-green-200">
                <h4 className="font-semibold text-green-800 mb-2">🎊 Congratulations! Your System is Live!</h4>
                <p className="text-sm text-green-700">
                  Your professional KeyClub member management system is ready for business. 
                  Cost: RM 0/month. Start growing your membership program today!
                </p>
              </div>
            </div>
          </div>
        )}

        {activeTab === 'members' && (
          <div>
            <div className="flex justify-between items-center mb-6">
              <h2 className="text-xl font-bold">Members Management</h2>
              <button
                onClick={() => setShowAddMember(true)}
                className="bg-purple-600 text-white px-4 py-2 rounded-lg hover:bg-purple-700 transition-colors"
              >
                Add Member
              </button>
            </div>

            <div className="bg-white rounded-lg shadow-sm overflow-hidden">
              <table className="w-full">
                <thead className="bg-gray-50">
                  <tr>
                    <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Member</th>
                    <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Branch</th>
                    <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Duration</th>
                    <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Status</th>
                    <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Actions</th>
                  </tr>
                </thead>
                <tbody className="divide-y divide-gray-200">
                  {members.map(member => (
                    <tr key={member.id}>
                      <td className="px-6 py-4">
                        <div>
                          <div className="font-medium text-gray-900">{member.name}</div>
                          <div className="text-sm text-gray-500">{member.email}</div>
                        </div>
                      </td>
                      <td className="px-6 py-4 text-sm text-gray-900">{member.branch}</td>
                      <td className="px-6 py-4 text-sm text-gray-900">{member.duration}</td>
                      <td className="px-6 py-4">
                        <span className={`px-2 py-1 text-xs rounded-full ${
                          member.status === 'Active' 
                            ? 'bg-green-100 text-green-800' 
                            : 'bg-orange-100 text-orange-800'
                        }`}>
                          {member.status}
                        </span>
                      </td>
                      <td className="px-6 py-4">
                        <button
                          onClick={() => setShowMemberCard(member)}
                          className="text-purple-600 hover:text-purple-800"
                        >
                          View Card
                        </button>
                      </td>
                    </tr>
                  ))}
                </tbody>
              </table>
            </div>
          </div>
        )}

        {activeTab === 'analytics' && (
          <div>
            <h2 className="text-xl font-bold mb-6">📊 Business Analytics</h2>
            
            <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
              <div className="bg-white p-6 rounded-lg shadow-sm border-l-4 border-green-500">
                <h3 className="text-lg font-semibold mb-2">Revenue</h3>
                <p className="text-2xl font-bold text-green-600">RM{totalRevenue}</p>
                <p className="text-sm text-gray-500">Total earnings</p>
              </div>
              
              <div className="bg-white p-6 rounded-lg shadow-sm border-l-4 border-blue-500">
                <h3 className="text-lg font-semibold mb-2">Members</h3>
                <p className="text-2xl font-bold text-blue-600">{totalMembers}</p>
                <p className="text-sm text-gray-500">{activeMembers} active</p>
              </div>
              
              <div className="bg-white p-6 rounded-lg shadow-sm border-l-4 border-purple-500">
                <h3 className="text-lg font-semibold mb-2">Health</h3>
                <p className="text-2xl font-bold text-purple-600">{totalMembers > 0 ? Math.round((activeMembers / totalMembers) * 100) : 0}%</p>
                <p className="text-sm text-gray-500">Active rate</p>
              </div>
            </div>
          </div>
        )}
      </div>

      {showAddMember && (
        <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4 z-50">
          <div className="bg-white rounded-lg p-6 w-full max-w-md">
            <h3 className="text-lg font-semibold mb-4">Add New Member</h3>
            
            <div className="space-y-4">
              <input
                type="text"
                placeholder="Full Name"
                value={newMember.name}
                onChange={(e) => setNewMember({...newMember, name: e.target.value})}
                className="w-full p-3 border border-gray-300 rounded-lg"
              />
              <input
                type="email"
                placeholder="Email"
                value={newMember.email}
                onChange={(e) => setNewMember({...newMember, email: e.target.value})}
                className="w-full p-3 border border-gray-300 rounded-lg"
              />
              <input
                type="tel"
                placeholder="Phone Number"
                value={newMember.phone}
                onChange={(e) => setNewMember({...newMember, phone: e.target.value})}
                className="w-full p-3 border border-gray-300 rounded-lg"
              />
              <input
                type="text"
                placeholder="Branch Name"
                value={newMember.branch}
                onChange={(e) => setNewMember({...newMember, branch: e.target.value})}
                className="w-full p-3 border border-gray-300 rounded-lg"
              />
              <select
                value={newMember.duration}
                onChange={(e) => setNewMember({...newMember, duration: e.target.value})}
                className="w-full p-3 border border-gray-300 rounded-lg"
              >
                {durationOptions.map(option => (
                  <option key={option.value} value={option.value}>
                    {option.value} - RM{option.price}
                  </option>
                ))}
              </select>
            </div>
            
            <div className="flex space-x-3 mt-6">
              <button
                onClick={() => setShowAddMember(false)}
                className="flex-1 py-2 px-4 border border-gray-300 rounded-lg hover:bg-gray-50"
              >
                Cancel
              </button>
              <button
                onClick={addMember}
                className="flex-1 py-2 px-4 bg-purple-600 text-white rounded-lg hover:bg-purple-700"
              >
                Add Member
              </button>
            </div>
          </div>
        </div>
      )}

      {showMemberCard && (
        <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4 z-50">
          <div className="bg-white rounded-lg p-6 max-w-sm w-full">
            <div className="flex justify-between items-center mb-4">
              <h3 className="text-lg font-semibold">Member Card</h3>
              <button
                onClick={() => setShowMemberCard(null)}
                className="text-gray-500 hover:text-gray-700 text-xl"
              >
                ×
              </button>
            </div>
            
            <div className="bg-gradient-to-br from-purple-600 via-pink-500 to-orange-400 p-6 rounded-2xl text-white shadow-2xl">
              <div className="flex justify-between items-start mb-4">
                <div>
                  <h3 className="text-xl font-bold">1+1.5 KeyClub</h3>
                  <p className="text-sm opacity-90">Premium Member</p>
                </div>
                <div className="bg-white/20 p-2 rounded-lg">
                  <img src={generateQRCode(showMemberCard.qrCode)} alt="QR Code" className="w-12 h-12" />
                </div>
              </div>
              
              <div className="mb-4">
                <h4 className="text-lg font-semibold">{showMemberCard.name}</h4>
                <p className="text-sm opacity-90">{showMemberCard.qrCode} • {showMemberCard.branch}</p>
              </div>
              
              <div className="grid grid-cols-2 gap-4 text-sm">
                <div>
                  <p className="opacity-75">Valid Until</p>
                  <p className="font-semibold">{new Date(showMemberCard.expiryDate).toLocaleDateString()}</p>
                </div>
                <div>
                  <p className="opacity-75">Referrals</p>
                  <p className="font-semibold">{showMemberCard.referrals}/3</p>
                </div>
              </div>
              
              <div className="mt-4 p-3 bg-white/20 rounded-lg">
                <p className="text-xs font-semibold mb-1">MEMBER BENEFITS:</p>
                <p className="text-xs">• RM1 Fried Rice (1 plate)</p>
                <p className="text-xs">• Refer 3 friends = +1 week free</p>
              </div>
            </div>
          </div>
        </div>
      )}
    </div>
  );
};

export default KeyClubApp;
