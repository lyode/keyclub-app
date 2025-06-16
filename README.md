import React, { useState, useEffect } from 'react';
import { Users, Calendar, Plus, QrCode, Bell, TrendingUp, Gift, UserPlus, Clock, DollarSign, BarChart3, MapPin, Trash2, UserX, Edit } from 'lucide-react';

const KeyClubApp = () => {
  const [activeTab, setActiveTab] = useState('dashboard');
  const [members, setMembers] = useState([
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
  ]);

  const [showAddMember, setShowAddMember] = useState(false);
  const [showMemberCard, setShowMemberCard] = useState(null);
  const [showEditMember, setShowEditMember] = useState(null);
  const [showDeleteConfirm, setShowDeleteConfirm] = useState(null);
  const [newMember, setNewMember] = useState({
    name: '',
    email: '',
    phone: '',
    branch: '',
    duration: '1 month'
  });

  const [editMember, setEditMember] = useState({
    id: null,
    name: '',
    email: '',
    phone: '',
    branch: '',
    duration: '1 month'
  });

  const [dailyStats, setDailyStats] = useState({
    newMembers: 3,
    totalCollection: 450,
    expiringToday: 1,
    activeMembers: members.filter(m => m.status === 'Active').length
  });

  const durationOptions = [
    { value: '1 month', price: 30 },
    { value: '3 months', price: 80 },
    { value: '6 months', price: 150 },
    { value: '1 year', price: 280 }
  ];

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

      const member = {
        id: members.length + 1,
        ...newMember,
        joinDate: joinDate.toISOString().split('T')[0],
        expiryDate: expiryDate.toISOString().split('T')[0],
        status: 'Active',
        referrals: 0,
        qrCode: `KC${String(members.length + 1).padStart(3, '0')}`
      };

      setMembers([...members, member]);
      setNewMember({ name: '', email: '', phone: '', branch: '', duration: '1 month' });
      setShowAddMember(false);
      
      // Update daily stats
      setDailyStats(prev => ({
        ...prev,
        newMembers: prev.newMembers + 1,
        totalCollection: prev.totalCollection + durationOptions.find(d => d.value === newMember.duration).price,
        activeMembers: prev.activeMembers + 1
      }));
    }
  };

  const updateMember = () => {
    if (editMember.name && editMember.email && editMember.phone && editMember.branch) {
      const updatedMembers = members.map(member => {
        if (member.id === editMember.id) {
          // Recalculate expiry date if duration changed
          const expiryDate = new Date(member.joinDate);
          switch(editMember.duration) {
            case '1 month': expiryDate.setMonth(expiryDate.getMonth() + 1); break;
            case '3 months': expiryDate.setMonth(expiryDate.getMonth() + 3); break;
            case '6 months': expiryDate.setMonth(expiryDate.getMonth() + 6); break;
            case '1 year': expiryDate.setFullYear(expiryDate.getFullYear() + 1); break;
          }
          
          return {
            ...member,
            name: editMember.name,
            email: editMember.email,
            phone: editMember.phone,
            branch: editMember.branch,
            duration: editMember.duration,
            expiryDate: expiryDate.toISOString().split('T')[0]
          };
        }
        return member;
      });
      
      setMembers(updatedMembers);
      setShowEditMember(null);
      setEditMember({ id: null, name: '', email: '', phone: '', branch: '', duration: '1 month' });
    }
  };

  const toggleMemberStatus = (memberId) => {
    const updatedMembers = members.map(member => {
      if (member.id === memberId) {
        return {
          ...member,
          status: member.status === 'Active' ? 'Inactive' : 'Active'
        };
      }
      return member;
    });
    setMembers(updatedMembers);
  };

  const deleteMember = (memberId) => {
    const updatedMembers = members.filter(member => member.id !== memberId);
    setMembers(updatedMembers);
    setShowDeleteConfirm(null);
    
    // Update daily stats
    setDailyStats(prev => ({
      ...prev,
      activeMembers: updatedMembers.filter(m => m.status === 'Active').length
    }));
  };

  const openEditMember = (member) => {
    setEditMember({
      id: member.id,
      name: member.name,
      email: member.email,
      phone: member.phone,
      branch: member.branch,
      duration: member.duration
    });
    setShowEditMember(true);
  };

  const generateQRCode = (qrCode) => {
    return `https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=${qrCode}`;
  };

  const MemberCard = ({ member }) => (
    <div className="bg-gradient-to-br from-purple-600 via-pink-500 to-orange-400 p-6 rounded-2xl text-white shadow-2xl max-w-sm mx-auto">
      <div className="flex justify-between items-start mb-4">
        <div>
          <h3 className="text-xl font-bold">1+1.5 KeyClub</h3>
          <p className="text-sm opacity-90">Premium Member</p>
        </div>
        <div className="bg-white/20 p-2 rounded-lg">
          <img src={generateQRCode(member.qrCode)} alt="QR Code" className="w-12 h-12" />
        </div>
      </div>
      
      <div className="mb-4">
        <h4 className="text-lg font-semibold">{member.name}</h4>
        <p className="text-sm opacity-90">{member.qrCode} • {member.branch}</p>
      </div>
      
      <div className="grid grid-cols-2 gap-4 text-sm">
        <div>
          <p className="opacity-75">Valid Until</p>
          <p className="font-semibold">{new Date(member.expiryDate).toLocaleDateString()}</p>
        </div>
        <div>
          <p className="opacity-75">Referrals</p>
          <p className="font-semibold">{member.referrals}/3</p>
        </div>
      </div>
      
      <div className="mt-4 p-3 bg-white/20 rounded-lg">
        <p className="text-xs font-semibold mb-1">MEMBER BENEFITS:</p>
        <p className="text-xs">• RM1 Fried Rice (1 plate)</p>
        <p className="text-xs">• Refer 3 friends = +1 week free</p>
      </div>
    </div>
  );

  const getDaysUntilExpiry = (expiryDate) => {
    const today = new Date();
    const expiry = new Date(expiryDate);
    const diffTime = expiry - today;
    return Math.ceil(diffTime / (1000 * 60 * 60 * 24));
  };

  return (
    <div className="min-h-screen bg-gray-50">
      {/* Header */}
      <div className="bg-gradient-to-r from-purple-600 to-pink-600 text-white p-4 shadow-lg">
        <h1 className="text-2xl font-bold">1+1.5 KeyClub Members</h1>
        <p className="text-sm opacity-90">Member Management System</p>
      </div>

      {/* Navigation */}
      <div className="bg-white shadow-sm">
        <div className="flex space-x-6 p-4">
          {[
            { id: 'dashboard', label: 'Dashboard', icon: TrendingUp },
            { id: 'members', label: 'Members', icon: Users },
            { id: 'analysis', label: 'Analysis', icon: BarChart3 },
            { id: 'alerts', label: 'Alerts', icon: Bell }
          ].map(tab => {
            const Icon = tab.icon;
            return (
              <button
                key={tab.id}
                onClick={() => setActiveTab(tab.id)}
                className={`flex items-center space-x-2 px-4 py-2 rounded-lg transition-colors ${
                  activeTab === tab.id 
                    ? 'bg-purple-100 text-purple-600' 
                    : 'text-gray-600 hover:bg-gray-100'
                }`}
              >
                <Icon size={20} />
                <span>{tab.label}</span>
              </button>
            );
          })}
        </div>
      </div>

      {/* Content */}
      <div className="p-6">
        {/* Dashboard */}
        {activeTab === 'dashboard' && (
          <div>
            <h2 className="text-xl font-bold mb-6">Daily Summary</h2>
            
            <div className="grid grid-cols-1 md:grid-cols-4 gap-6 mb-8">
              <div className="bg-white p-6 rounded-lg shadow-sm border-l-4 border-green-500">
                <div className="flex items-center justify-between">
                  <div>
                    <p className="text-sm text-gray-600">New Members Today</p>
                    <p className="text-2xl font-bold text-green-600">{dailyStats.newMembers}</p>
                  </div>
                  <UserPlus className="text-green-500" size={24} />
                </div>
              </div>
              
              <div className="bg-white p-6 rounded-lg shadow-sm border-l-4 border-blue-500">
                <div className="flex items-center justify-between">
                  <div>
                    <p className="text-sm text-gray-600">Total Collection</p>
                    <p className="text-2xl font-bold text-blue-600">RM{dailyStats.totalCollection}</p>
                  </div>
                  <DollarSign className="text-blue-500" size={24} />
                </div>
              </div>
              
              <div className="bg-white p-6 rounded-lg shadow-sm border-l-4 border-orange-500">
                <div className="flex items-center justify-between">
                  <div>
                    <p className="text-sm text-gray-600">Expiring Today</p>
                    <p className="text-2xl font-bold text-orange-600">{dailyStats.expiringToday}</p>
                  </div>
                  <Clock className="text-orange-500" size={24} />
                </div>
              </div>
              
              <div className="bg-white p-6 rounded-lg shadow-sm border-l-4 border-purple-500">
                <div className="flex items-center justify-between">
                  <div>
                    <p className="text-sm text-gray-600">Active Members</p>
                    <p className="text-2xl font-bold text-purple-600">{dailyStats.activeMembers}</p>
                  </div>
                  <Users className="text-purple-500" size={24} />
                </div>
              </div>
            </div>

            <div className="bg-white p-6 rounded-lg shadow-sm">
              <h3 className="text-lg font-semibold mb-4">Member Benefits</h3>
              <div className="space-y-3">
                <div className="flex items-center space-x-3">
                  <Gift className="text-purple-500" size={20} />
                  <span>RM1 only for 1 plate fried rice</span>
                </div>
                <div className="flex items-center space-x-3">
                  <UserPlus className="text-green-500" size={20} />
                  <span>Introduce 3 members = Prolong membership 1 week</span>
                </div>
              </div>
            </div>
          </div>
        )}

        {/* Members */}
        {activeTab === 'members' && (
          <div>
            <div className="flex justify-between items-center mb-6">
              <h2 className="text-xl font-bold">Members Management</h2>
              <button
                onClick={() => setShowAddMember(true)}
                className="bg-purple-600 text-white px-4 py-2 rounded-lg flex items-center space-x-2 hover:bg-purple-700 transition-colors"
              >
                <Plus size={20} />
                <span>Add Member</span>
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
                    <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Referrals</th>
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
                      <td className="px-6 py-4">
                        <div className="flex items-center space-x-1">
                          <MapPin size={14} className="text-gray-400" />
                          <span className="text-sm text-gray-900">{member.branch}</span>
                        </div>
                      </td>
                      <td className="px-6 py-4 text-sm text-gray-900">{member.duration}</td>
                      <td className="px-6 py-4">
                        <span className={`px-2 py-1 text-xs rounded-full ${
                          member.status === 'Active' 
                            ? 'bg-green-100 text-green-800' 
                            : member.status === 'Expiring Soon'
                            ? 'bg-orange-100 text-orange-800'
                            : 'bg-red-100 text-red-800'
                        }`}>
                          {member.status}
                        </span>
                      </td>
                      <td className="px-6 py-4 text-sm text-gray-900">
                        {member.referrals}/3
                        {member.referrals >= 3 && (
                          <span className="ml-2 text-xs bg-green-100 text-green-800 px-2 py-1 rounded">
                            +1 week earned
                          </span>
                        )}
                      </td>
                      <td className="px-6 py-4">
                        <div className="flex space-x-2">
                          <button
                            onClick={() => setShowMemberCard(member)}
                            className="text-purple-600 hover:text-purple-800 flex items-center space-x-1"
                          >
                            <QrCode size={16} />
                            <span className="text-sm">Card</span>
                          </button>
                          <button
                            onClick={() => openEditMember(member)}
                            className="text-blue-600 hover:text-blue-800 flex items-center space-x-1"
                          >
                            <Edit size={16} />
                            <span className="text-sm">Edit</span>
                          </button>
                          <button
                            onClick={() => toggleMemberStatus(member.id)}
                            className={`${member.status === 'Active' ? 'text-orange-600 hover:text-orange-800' : 'text-green-600 hover:text-green-800'} flex items-center space-x-1`}
                          >
                            <UserX size={16} />
                            <span className="text-sm">{member.status === 'Active' ? 'Deactivate' : 'Activate'}</span>
                          </button>
                          <button
                            onClick={() => setShowDeleteConfirm(member)}
                            className="text-red-600 hover:text-red-800 flex items-center space-x-1"
                          >
                            <Trash2 size={16} />
                            <span className="text-sm">Delete</span>
                          </button>
                        </div>
                      </td>
                    </tr>
                  ))}
                </tbody>
              </table>
            </div>
          </div>
        )}

        {/* Analysis */}
        {activeTab === 'analysis' && (
          <div>
            <h2 className="text-xl font-bold mb-6">Member Analysis</h2>
            
            {(() => {
              // Calculate statistics
              const totalMembers = members.length;
              const activeMembers = members.filter(m => m.status === 'Active').length;
              const totalRevenue = members.reduce((sum, member) => {
                const price = durationOptions.find(d => d.value === member.duration)?.price || 0;
                return sum + price;
              }, 0);
              
              // Branch analysis
              const branchStats = members.reduce((acc, member) => {
                acc[member.branch] = (acc[member.branch] || 0) + 1;
                return acc;
              }, {});
              
              // Duration analysis
              const durationStats = members.reduce((acc, member) => {
                acc[member.duration] = (acc[member.duration] || 0) + 1;
                return acc;
              }, {});
              
              // Revenue by branch
              const branchRevenue = members.reduce((acc, member) => {
                const price = durationOptions.find(d => d.value === member.duration)?.price || 0;
                acc[member.branch] = (acc[member.branch] || 0) + price;
                return acc;
              }, {});
              
              return (
                <div className="space-y-6">
                  {/* Summary Cards */}
                  <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <div className="bg-white p-6 rounded-lg shadow-sm border-l-4 border-blue-500">
                      <h3 className="text-lg font-semibold text-gray-900 mb-2">Total Members</h3>
                      <p className="text-3xl font-bold text-blue-600">{totalMembers}</p>
                      <p className="text-sm text-gray-500 mt-1">{activeMembers} Active</p>
                    </div>
                    
                    <div className="bg-white p-6 rounded-lg shadow-sm border-l-4 border-green-500">
                      <h3 className="text-lg font-semibold text-gray-900 mb-2">Total Revenue</h3>
                      <p className="text-3xl font-bold text-green-600">RM{totalRevenue}</p>
                      <p className="text-sm text-gray-500 mt-1">All time earnings</p>
                    </div>
                    
                    <div className="bg-white p-6 rounded-lg shadow-sm border-l-4 border-purple-500">
                      <h3 className="text-lg font-semibold text-gray-900 mb-2">Avg. Revenue</h3>
                      <p className="text-3xl font-bold text-purple-600">RM{Math.round(totalRevenue / totalMembers)}</p>
                      <p className="text-sm text-gray-500 mt-1">Per member</p>
                    </div>
                  </div>
                  
                  {/* Branch Analysis */}
                  <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
                    <div className="bg-white p-6 rounded-lg shadow-sm">
                      <h3 className="text-lg font-semibold mb-4">Members by Branch</h3>
                      <div className="space-y-3">
                        {Object.entries(branchStats).map(([branch, count]) => (
                          <div key={branch} className="flex justify-between items-center">
                            <div className="flex items-center space-x-2">
                              <MapPin size={16} className="text-gray-400" />
                              <span className="font-medium">{branch}</span>
                            </div>
                            <div className="flex items-center space-x-3">
                              <span className="text-sm text-gray-600">{count} members</span>
                              <div className="w-20 bg-gray-200 rounded-full h-2">
                                <div 
                                  className="bg-purple-600 h-2 rounded-full" 
                                  style={{width: `${(count / totalMembers) * 100}%`}}
                                ></div>
                              </div>
                              <span className="text-sm font-semibold text-purple-600">
                                {Math.round((count / totalMembers) * 100)}%
                              </span>
                            </div>
                          </div>
                        ))}
                      </div>
                    </div>
                    
                    <div className="bg-white p-6 rounded-lg shadow-sm">
                      <h3 className="text-lg font-semibold mb-4">Revenue by Branch</h3>
                      <div className="space-y-3">
                        {Object.entries(branchRevenue).map(([branch, revenue]) => (
                          <div key={branch} className="flex justify-between items-center">
                            <div className="flex items-center space-x-2">
                              <DollarSign size={16} className="text-gray-400" />
                              <span className="font-medium">{branch}</span>
                            </div>
                            <div className="flex items-center space-x-3">
                              <span className="text-sm text-gray-600">RM{revenue}</span>
                              <div className="w-20 bg-gray-200 rounded-full h-2">
                                <div 
                                  className="bg-green-600 h-2 rounded-full" 
                                  style={{width: `${(revenue / totalRevenue) * 100}%`}}
                                ></div>
                              </div>
                              <span className="text-sm font-semibold text-green-600">
                                {Math.round((revenue / totalRevenue) * 100)}%
                              </span>
                            </div>
                          </div>
                        ))}
                      </div>
                    </div>
                  </div>
                  
                  {/* Duration Analysis */}
                  <div className="bg-white p-6 rounded-lg shadow-sm">
                    <h3 className="text-lg font-semibold mb-4">Membership Duration Preferences</h3>
                    <div className="grid grid-cols-2 md:grid-cols-4 gap-4">
                      {Object.entries(durationStats).map(([duration, count]) => (
                        <div key={duration} className="text-center p-4 bg-gray-50 rounded-lg">
                          <p className="text-2xl font-bold text-purple-600">{count}</p>
                          <p className="text-sm text-gray-600">{duration}</p>
                          <p className="text-xs text-gray-500 mt-1">
                            {Math.round((count / totalMembers) * 100)}% of members
                          </p>
                        </div>
                      ))}
                    </div>
                  </div>
                  
                  {/* Referral Analysis */}
                  <div className="bg-white p-6 rounded-lg shadow-sm">
                    <h3 className="text-lg font-semibold mb-4">Referral Program Performance</h3>
                    <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
                      <div className="text-center p-4 bg-blue-50 rounded-lg">
                        <p className="text-2xl font-bold text-blue-600">
                          {members.reduce((sum, member) => sum + member.referrals, 0)}
                        </p>
                        <p className="text-sm text-gray-600">Total Referrals</p>
                      </div>
                      <div className="text-center p-4 bg-green-50 rounded-lg">
                        <p className="text-2xl font-bold text-green-600">
                          {members.filter(member => member.referrals >= 3).length}
                        </p>
                        <p className="text-sm text-gray-600">Earned Free Week</p>
                      </div>
                      <div className="text-center p-4 bg-orange-50 rounded-lg">
                        <p className="text-2xl font-bold text-orange-600">
                          {Math.round((members.reduce((sum, member) => sum + member.referrals, 0) / totalMembers) * 10) / 10}
                        </p>
                        <p className="text-sm text-gray-600">Avg. Referrals/Member</p>
                      </div>
                    </div>
                  </div>
                </div>
              );
            })()}
          </div>
        )}

        {/* Alerts */}
        {activeTab === 'alerts' && (
          <div>
            <h2 className="text-xl font-bold mb-6">Membership Alerts</h2>
            
            <div className="space-y-4">
              {members.filter(member => getDaysUntilExpiry(member.expiryDate) <= 7).map(member => (
                <div key={member.id} className="bg-white p-4 rounded-lg shadow-sm border-l-4 border-orange-500">
                  <div className="flex justify-between items-center">
                    <div>
                      <h3 className="font-semibold text-gray-900">{member.name}</h3>
                      <p className="text-sm text-gray-600">
                        Membership expires in {getDaysUntilExpiry(member.expiryDate)} days
                      </p>
                    </div>
                    <div className="text-right">
                      <p className="text-sm text-gray-500">Expires: {new Date(member.expiryDate).toLocaleDateString()}</p>
                      <button className="text-purple-600 text-sm hover:underline">Send Reminder</button>
                    </div>
                  </div>
                </div>
              ))}
            </div>
          </div>
        )}
      </div>

      {/* Add Member Modal */}
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
                className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent"
              />
              
              <input
                type="email"
                placeholder="Email"
                value={newMember.email}
                onChange={(e) => setNewMember({...newMember, email: e.target.value})}
                className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent"
              />
              
              <input
                type="tel"
                placeholder="Phone Number"
                value={newMember.phone}
                onChange={(e) => setNewMember({...newMember, phone: e.target.value})}
                className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent"
              />
              
              <input
                type="text"
                placeholder="Branch Name"
                value={newMember.branch}
                onChange={(e) => setNewMember({...newMember, branch: e.target.value})}
                className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent"
              />
              
              <select
                value={newMember.duration}
                onChange={(e) => setNewMember({...newMember, duration: e.target.value})}
                className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent"
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
                className="flex-1 py-2 px-4 border border-gray-300 rounded-lg hover:bg-gray-50 transition-colors"
              >
                Cancel
              </button>
              <button
                onClick={addMember}
                className="flex-1 py-2 px-4 bg-purple-600 text-white rounded-lg hover:bg-purple-700 transition-colors"
              >
                Add Member
              </button>
            </div>
          </div>
        </div>
      )}

      {/* Edit Member Modal */}
      {showEditMember && (
        <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4 z-50">
          <div className="bg-white rounded-lg p-6 w-full max-w-md">
            <h3 className="text-lg font-semibold mb-4">Edit Member</h3>
            
            <div className="space-y-4">
              <input
                type="text"
                placeholder="Full Name"
                value={editMember.name}
                onChange={(e) => setEditMember({...editMember, name: e.target.value})}
                className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent"
              />
              
              <input
                type="email"
                placeholder="Email"
                value={editMember.email}
                onChange={(e) => setEditMember({...editMember, email: e.target.value})}
                className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent"
              />
              
              <input
                type="tel"
                placeholder="Phone Number"
                value={editMember.phone}
                onChange={(e) => setEditMember({...editMember, phone: e.target.value})}
                className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent"
              />
              
              <input
                type="text"
                placeholder="Branch Name"
                value={editMember.branch}
                onChange={(e) => setEditMember({...editMember, branch: e.target.value})}
                className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent"
              />
              
              <select
                value={editMember.duration}
                onChange={(e) => setEditMember({...editMember, duration: e.target.value})}
                className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent"
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
                onClick={() => setShowEditMember(false)}
                className="flex-1 py-2 px-4 border border-gray-300 rounded-lg hover:bg-gray-50 transition-colors"
              >
                Cancel
              </button>
              <button
                onClick={updateMember}
                className="flex-1 py-2 px-4 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors"
              >
                Update Member
              </button>
            </div>
          </div>
        </div>
      )}

      {/* Delete Confirmation Modal */}
      {showDeleteConfirm && (
        <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4 z-50">
          <div className="bg-white rounded-lg p-6 w-full max-w-md">
            <h3 className="text-lg font-semibold mb-4 text-red-600">Delete Member</h3>
            <p className="text-gray-700 mb-6">
              Are you sure you want to permanently delete <strong>{showDeleteConfirm.name}</strong>? 
              This action cannot be undone.
            </p>
            
            <div className="flex space-x-3">
              <button
                onClick={() => setShowDeleteConfirm(null)}
                className="flex-1 py-2 px-4 border border-gray-300 rounded-lg hover:bg-gray-50 transition-colors"
              >
                Cancel
              </button>
              <button
                onClick={() => deleteMember(showDeleteConfirm.id)}
                className="flex-1 py-2 px-4 bg-red-600 text-white rounded-lg hover:bg-red-700 transition-colors"
              >
                Delete
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
                className="text-gray-500 hover:text-gray-700"
              >
                ✕
              </button>
            </div>
            
            <MemberCard member={showMemberCard} />
            
            <div className="mt-4 space-y-2">
              <button className="w-full py-2 px-4 bg-purple-600 text-white rounded-lg hover:bg-purple-700 transition-colors">
                Download Card
              </button>
              <button className="w-full py-2 px-4 border border-gray-300 rounded-lg hover:bg-gray-50 transition-colors">
                Send via WhatsApp
              </button>
            </div>
          </div>
        </div>
      )}
    </div>
  );
};

export default KeyClubApp;
