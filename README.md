import React, { useState, useEffect } from 'react';
import { Camera, Share2 } from 'lucide-react';
import { Button } from '@/components/ui/button';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { Input } from '@/components/ui/input';

const challenges = [
  "كوب قهوة", "نبات منزلي", "كتاب", "حذاء", "نظارة شمسية", "ساعة",
  "هاتف ذكي", "مفتاح", "قلم", "شوكة", "ملعقة", "منشفة",
  "وسادة", "مصباح", "لوحة فنية", "فرشاة أسنان", "مرآة", "حقيبة",
  "سماعات رأس", "شاحن هاتف", "مظلة", "قبعة", "وشاح", "قفازات",
  "مجلة", "تقويم", "طابع بريدي", "بطاقة ائتمان", "عملة معدنية", "زجاجة ماء",
  "سلة مهملات", "مشبك ورق", "دبوس", "شريط لاصق", "مقص", "خيط",
  "إبرة", "زر", "منديل", "علبة مناديل"
];

const PhotoChallengeGame = () => {
  const [selectedNumber, setSelectedNumber] = useState('');
  const [currentChallenge, setCurrentChallenge] = useState(null);

  useEffect(() => {
    const params = new URLSearchParams(window.location.search);
    const challengeNumber = params.get('challenge');
    if (challengeNumber) {
      const number = parseInt(challengeNumber, 10);
      if (number >= 1 && number <= 40) {
        setSelectedNumber(challengeNumber);
        setCurrentChallenge({
          number: number,
          item: challenges[number - 1]
        });
      }
    }
  }, []);

  const handleNumberChange = (e) => {
    setSelectedNumber(e.target.value);
  };

  const startChallenge = () => {
    const number = parseInt(selectedNumber, 10);
    if (number >= 1 && number <= 40) {
      setCurrentChallenge({
        number: number,
        item: challenges[number - 1]
      });
      // Update URL
      window.history.pushState({}, '', `?challenge=${number}`);
    } else {
      alert('الرجاء إدخال رقم بين 1 و 40');
    }
  };

  const shareChallenge = () => {
    if (currentChallenge) {
      const shareUrl = `${window.location.origin}${window.location.pathname}?challenge=${currentChallenge.number}`;
      navigator.clipboard.writeText(shareUrl).then(() => {
        alert('تم نسخ رابط التحدي إلى الحافظة!');
      });
    }
  };

  return (
    <Card className="w-[300px] mx-auto">
      <CardHeader>
        <CardTitle className="text-center">تحدي التصوير المخصص</CardTitle>
      </CardHeader>
      <CardContent>
        <div className="flex flex-col items-center space-y-4">
          <Input
            type="number"
            min="1"
            max="40"
            value={selectedNumber}
            onChange={handleNumberChange}
            placeholder="اختر رقماً من 1 إلى 40"
            className="text-center"
          />
          <Button onClick={startChallenge}>ابدأ التحدي</Button>
          {currentChallenge && (
            <div className="text-center">
              <p className="text-xl font-bold mb-2">الرقم: {currentChallenge.number}</p>
              <p className="text-lg">صور: {currentChallenge.item}</p>
              <Camera className="mx-auto mt-4" size={48} />
              <Button onClick={shareChallenge} className="mt-4">
                <Share2 className="mr-2 h-4 w-4" /> مشاركة التحدي
              </Button>
            </div>
          )}
        </div>
      </CardContent>
    </Card>
  );
};

export default PhotoChallengeGame;
