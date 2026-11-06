

```dart

import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Internship Opportunity',
      theme: ThemeData(
        colorSchemeSeed: Colors.deepPurple,
        useMaterial3: true,
      ),
      home: const InternshipHome(),
    );
  }
}

class Internship {
  final String title;
  final String company;
  final String location;
  final String duration;
  final String stipend;
  final String category;

  const Internship({
    required this.title,
    required this.company,
    required this.location,
    required this.duration,
    required this.stipend,
    required this.category,
  });
}

class InternshipHome extends StatefulWidget {
  const InternshipHome({super.key});

  @override
  State<InternshipHome> createState() => _InternshipHomeState();
}

class _InternshipHomeState extends State<InternshipHome> {
  final TextEditingController search = TextEditingController();

  String category = 'All';

  final List<Internship> jobs = [
    const Internship(
      title: 'Flutter Developer',
      company: 'Google',
      location: 'Remote',
      duration: '3 Months',
      stipend: '₹10,000',
      category: 'Flutter',
    ),
    const Internship(
      title: 'Python Developer',
      company: 'Infosys',
      location: 'Bangalore',
      duration: '2 Months',
      stipend: '₹8,000',
      category: 'Python',
    ),
    const Internship(
      title: 'Web Developer',
      company: 'TCS',
      location: 'Hyderabad',
      duration: '3 Months',
      stipend: '₹12,000',
      category: 'Web',
    ),
    const Internship(
      title: 'Java Developer',
      company: 'Wipro',
      location: 'Remote',
      duration: '2 Months',
      stipend: '₹7,000',
      category: 'Java',
    ),
    const Internship(
      title: 'AI Intern',
      company: 'Microsoft',
      location: 'Remote',
      duration: '4 Months',
      stipend: '₹15,000',
      category: 'AI',
    ),
  ];

  final List<Internship> applied = [];

  List<Internship> get filteredJobs {
    return jobs.where((job) {
      bool matchesCategory =
          category == 'All' || job.category == category;

      bool matchesSearch =
          job.title.toLowerCase().contains(
                search.text.toLowerCase(),
              ) ||
          job.company.toLowerCase().contains(
                search.text.toLowerCase(),
              );

      return matchesCategory && matchesSearch;
    }).toList();
  }

  void apply(Internship job) {
    if (applied.contains(job)) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('You have already applied!'),
        ),
      );
      return;
    }

    setState(() {
      applied.add(job);
    });

    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(
        content: Text('Application submitted successfully!'),
      ),
    );
  }

  void showJobDetails(Internship job) {
    showDialog(
      context: context,
      builder: (context) {
        return AlertDialog(
          title: Text(job.title),
          content: Column(
            mainAxisSize: MainAxisSize.min,
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text('Company: ${job.company}'),
              Text('Location: ${job.location}'),
              Text('Duration: ${job.duration}'),
              Text('Stipend: ${job.stipend}'),
              Text('Category: ${job.category}'),
            ],
          ),
          actions: [
            TextButton(
              onPressed: () => Navigator.pop(context),
              child: const Text('Close'),
            ),
            ElevatedButton(
              onPressed: () {
                Navigator.pop(context);
                apply(job);
              },
              child: const Text('Apply Now'),
            ),
          ],
        );
      },
    );
  }

  void showApplications() {
    Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => ApplicationPage(
          applied: applied,
        ),
      ),
    ).then((_) => setState(() {}));
  }

  @override
  void dispose() {
    search.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text(
          'Internship Finder',
          style: TextStyle(
            color: Colors.white,
            fontWeight: FontWeight.bold,
          ),
        ),
        backgroundColor: Colors.deepPurple,
        actions: [
          IconButton(
            icon: const Icon(
              Icons.assignment,
              color: Colors.white,
            ),
            onPressed: showApplications,
          ),
        ],
      ),

      body: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Container(
            width: double.infinity,
            padding: const EdgeInsets.all(20),
            color: Colors.deepPurple.shade50,
            child: const Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  'Find Your Dream Internship!',
                  style: TextStyle(
                    fontSize: 23,
                    fontWeight: FontWeight.bold,
                  ),
                ),
                SizedBox(height: 8),
                Text('Discover opportunities and build your career.'),
              ],
            ),
          ),

          Padding(
            padding: const EdgeInsets.all(15),
            child: TextField(
              controller: search,
              onChanged: (value) => setState(() {}),
              decoration: InputDecoration(
                hintText: 'Search internship or company',
                prefixIcon: const Icon(Icons.search),
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(15),
                ),
              ),
            ),
          ),

          const Padding(
            padding: EdgeInsets.symmetric(horizontal: 15),
            child: Text(
              'Categories',
              style: TextStyle(
                fontSize: 19,
                fontWeight: FontWeight.bold,
              ),
            ),
          ),

          const SizedBox(height: 10),

          SizedBox(
            height: 45,
            child: ListView(
              scrollDirection: Axis.horizontal,
              padding: const EdgeInsets.symmetric(horizontal: 10),
              children: [
                'All',
                'Flutter',
                'Python',
                'Web',
                'Java',
                'AI',
              ].map((item) {
                return Padding(
                  padding: const EdgeInsets.symmetric(horizontal: 4),
                  child: ChoiceChip(
                    label: Text(item),
                    selected: category == item,
                    onSelected: (value) {
                      setState(() {
                        category = item;
                      });
                    },
                  ),
                );
              }).toList(),
            ),
          ),

          const Padding(
            padding: EdgeInsets.all(15),
            child: Text(
              'Available Opportunities',
              style: TextStyle(
                fontSize: 19,
                fontWeight: FontWeight.bold,
              ),
            ),
          ),

          Expanded(
            child: filteredJobs.isEmpty
                ? const Center(
                    child: Text('No internships found!'),
                  )
                : ListView.builder(
                    itemCount: filteredJobs.length,
                    itemBuilder: (context, index) {
                      final job = filteredJobs[index];

                      return Card(
                        margin: const EdgeInsets.symmetric(
                          horizontal: 15,
                          vertical: 7,
                        ),
                        elevation: 3,
                        child: ListTile(
                          leading: const CircleAvatar(
                            backgroundColor: Colors.deepPurple,
                            child: Icon(
                              Icons.work,
                              color: Colors.white,
                            ),
                          ),
                          title: Text(
                            job.title,
                            style: const TextStyle(
                              fontWeight: FontWeight.bold,
                            ),
                          ),
                          subtitle: Text(
                            '${job.company}\n'
                            '${job.location} | ${job.duration}\n'
                            'Stipend: ${job.stipend}',
                          ),
                          isThreeLine: true,
                          trailing: const Icon(
                            Icons.arrow_forward_ios,
                            size: 18,
                          ),
                          onTap: () => showJobDetails(job),
                        ),
                      );
                    },
                  ),
          ),
        ],
      ),

      bottomNavigationBar: BottomNavigationBar(
        currentIndex: 0,
        selectedItemColor: Colors.deepPurple,
        onTap: (index) {
          if (index == 1) {
            showApplications();
          }
        },
        items: const [
          BottomNavigationBarItem(
            icon: Icon(Icons.home),
            label: 'Home',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.assignment),
            label: 'Applications',
          ),
        ],
      ),
    );
  }
}

class ApplicationPage extends StatelessWidget {
  final List<Internship> applied;

  const ApplicationPage({
    super.key,
    required this.applied,
  });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('My Applications'),
        backgroundColor: Colors.deepPurple,
        foregroundColor: Colors.white,
      ),
      body: applied.isEmpty
          ? const Center(
              child: Text(
                'No applications submitted yet!',
                style: TextStyle(fontSize: 18),
              ),
            )
          : ListView.builder(
              itemCount: applied.length,
              itemBuilder: (context, index) {
                final job = applied[index];

                return Card(
                  margin: const EdgeInsets.all(12),
                  child: ListTile(
                    leading: const Icon(
                      Icons.check_circle,
                      color: Colors.green,
                    ),
                    title: Text(job.title),
                    subtitle: Text(job.company),
                    trailing: const Text(
                      'Applied',
                      style: TextStyle(color: Colors.green),
                    ),
                  ),
                );
              },
            ),
    );
  }
}
```
