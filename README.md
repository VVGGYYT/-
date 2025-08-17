
import React, { useState } from "react";
import { SafeAreaView, Text, TextInput, Button, View } from "react-native";
import { NavigationContainer } from "@react-navigation/native";
import { createStackNavigator } from "@react-navigation/stack";

const Stack = createStackNavigator();

// 🏠 الشاشة الرئيسية
function HomeScreen({ navigation }) {
  return (
    <SafeAreaView style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
      <Text style={{ fontSize: 24, fontWeight: "bold" }}>📘 تطبيق الدراسة الذكي</Text>
      <Button title="ابدأ الاختبار" onPress={() => navigation.navigate("Quiz")} />
    </SafeAreaView>
  );
}

// 📝 شاشة الأسئلة
function QuizScreen({ navigation }) {
  const [answer, setAnswer] = useState("");
  const question = "حل 2x + 3 = 7";

  const handleSubmit = async () => {
    try {
      // 🚀 إرسال الجواب إلى خادم Node.js اللي يستخدم OpenAI
      const res = await fetch("http://localhost:3000/evaluate", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ question, answer }),
      });
      const data = await res.json();
      navigation.navigate("Result", { feedback: data.feedback });
    } catch (err) {
      console.error(err);
    }
  };

  return (
    <SafeAreaView style={{ flex: 1, padding: 20 }}>
      <Text style={{ fontSize: 20 }}>{question}</Text>
      <TextInput
        style={{ borderWidth: 1, marginVertical: 20, padding: 10 }}
        placeholder="اكتب الحل هنا"
        value={answer}
        onChangeText={setAnswer}
      />
      <Button title="إرسال" onPress={handleSubmit} />
    </SafeAreaView>
  );
}

// ✅ شاشة النتيجة
function ResultScreen({ route }) {
  const { feedback } = route.params;
  return (
    <SafeAreaView style={{ flex: 1, alignItems: "center", justifyContent: "center", padding: 20 }}>
      <Text style={{ fontSize: 20, marginBottom: 20 }}>📊 النتيجة:</Text>
      <Text style={{ fontSize: 18 }}>{feedback}</Text>
    </SafeAreaView>
  );
}

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Quiz" component={QuizScreen} />
        <Stack.Screen name="Result" component={ResultScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

/* 🌐 الكود الخلفي (Node.js + Express) – تحتاج تحفظه بملف server.js

const express = require("express");
const bodyParser = require("body-parser");
const cors = require("cors");
const { Configuration, OpenAIApi } = require("openai");

const app = express();
app.use(cors());
app.use(bodyParser.json());

const configuration = new Configuration({ apiKey: process.env.OPENAI_API_KEY });
const openai = new OpenAIApi(configuration);

app.post("/evaluate", async (req, res) => {
  const { question, answer } = req.body;
  const# -
منهج
