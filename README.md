
# chatconti
A site that allows you to talk to whomever you want just by using a code. Less internet, more talk.

"use client";

import React, { useState } from "react";
import { Button } from "@/components/ui/button";

const ProfilePage = () => {
  const [username, setUsername] = useState("Your Name"); // اسم المستخدم
  const [profileImage, setProfileImage] = useState<string | null>(null); // صورة البروفايل
  const [storyImage, setStoryImage] = useState<string | null>(null); // صورة الستوري
  const [storyText, setStoryText] = useState(""); // نص الستوري
  const [stories, setStories] = useState<{ text: string; image: string | null }[]>([]); // ستوريات المستخدم
  const [chatName, setChatName] = useState(""); // اسم المحادثة

  // دالة لتحميل صورة البروفايل
  const handleProfileImageChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    if (event.target.files && event.target.files[0]) {
      const file = event.target.files[0];
      const reader = new FileReader();
      reader.onloadend = () => {
        setProfileImage(reader.result as string);
      };
      reader.readAsDataURL(file);
    }
  };

  // دالة لتحميل صورة الستوري
  const handleStoryImageChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    if (event.target.files && event.target.files[0]) {
      const file = event.target.files[0];
      const reader = new FileReader();
      reader.onloadend = () => {
        setStoryImage(reader.result as string);
      };
      reader.readAsDataURL(file);
    }
  };

  // دالة لإضافة ستوري جديد
  const handleAddStory = () => {
    setStories([...stories, { text: storyText, image: storyImage }]);
    setStoryText("");
    setStoryImage(null);
  };

  // دالة لتغيير اسم المحادثة
  const handleChangeChatName = (e: React.ChangeEvent<HTMLInputElement>) => {
    setChatName(e.target.value);
  };

  return (
    <div className="flex flex-col items-center p-6 space-y-6">
      <h2 className="text-2xl font-semibold text-gray-700">My Profile</h2>
      
      {/* حقل لتغيير الاسم */}
      <div className="flex flex-col items-center">
        <label className="text-lg font-medium">Name</label>
        <input
          type="text"
          value={username}
          onChange={(e) => setUsername(e.target.value)}
          className="p-2 mt-2 border rounded-md focus:outline-none"
        />
      </div>

      {/* تحميل صورة البروفايل */}
      <div className="flex flex-col items-center">
        <label className="text-lg font-medium">Profile Picture</label>
        <label htmlFor="profile-image-upload" className="cursor-pointer mt-2">
          <img
            src={profileImage || "https://via.placeholder.com/100"}
            alt="Profile"
            className="w-24 h-24 rounded-full border"
          />
        </label>
        <input
          id="profile-image-upload"
          type="file"
          accept="image/*"
          className="hidden"
          onChange={handleProfileImageChange}
        />
      </div>

      {/* إنشاء ستوري */}
      <div className="flex flex-col items-center mt-6">
        <label className="text-lg font-medium">Create Your Story</label>

        {/* إضافة نص الستوري */}
        <textarea
          placeholder="Write your story..."
          value={storyText}
          onChange={(e) => setStoryText(e.target.value)}
          className="mt-2 p-2 border rounded-md focus:outline-none"
        />
        
        {/* إضافة صورة الستوري */}
        <label htmlFor="story-image-upload" className="cursor-pointer mt-2">
          {storyImage ? (
            <img src={storyImage} alt="Story" className="w-40 h-40 rounded-lg mt-2" />
          ) : (
            <div className="w-40 h-40 bg-gray-200 flex items-center justify-center rounded-lg text-gray-500">
              <span>Upload Story Image</span>
            </div>
          )}
        </label>
        <input
          id="story-image-upload"
          type="file"
          accept="image/*"
          className="hidden"
          onChange={handleStoryImageChange}
        />

        {/* زر إضافة الستوري */}
        <Button onClick={handleAddStory} className="mt-4 bg-blue-500 text-white">Add Story</Button>
      </div>

      {/* عرض الستوريات */}
      <div className="mt-8 w-full">
        <h3 className="text-lg font-semibold text-gray-700">My Stories</h3>
        <div className="flex space-x-4 overflow-x-auto mt-4">
          {stories.map((story, index) => (
            <div key={index} className="flex flex-col items-center space-y-2">
              {story.image ? (
                <img src={story.image} alt={`Story ${index}`} className="w-32 h-32 rounded-lg" />
              ) : (
                <div className="w-32 h-32 bg-gray-200 flex items-center justify-center rounded-lg text-gray-500">
                  <span>No Image</span>
                </div>
              )}
              <p className="text-sm text-gray-600">{story.text}</p>
            </div>
          ))}
        </div>
      </div>

      {/* تخصيص اسم المحادثة */}
      <div className="mt-6 w-full">
        <h3 className="text-lg font-semibold text-gray-700">Chat Customization</h3>
        <label className="text-md font-medium">Set Chat Name</label>
        <input
          type="text"
          placeholder="Set a name for your chat"
          value={chatName}
          onChange={handleChangeChatName}
          className="p-2 mt-2 border rounded-md focus:outline-none"
        />
      </div>
    </div>
  );
};

export default ProfilePage;
