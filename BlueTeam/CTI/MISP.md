# MISP Comprehensive Overview

## Dashboard and Navigation
- **Home Button**: Returns to the starting screen or custom home page
- **Event Actions**: Access to creation, modification, deletion, publishing and searching of events
- **Dashboard**: Customizable using widgets 
- **Galaxies**: Access to the list of MISP Galaxies for enhanced threat intelligence
- **Input Filters**: Shows validation rules and blocklists for data entry
- **Global Actions**: Access to information about MISP and the current instance
- **User Profile**: Access to personal settings and notifications

## Event Management Process
1. **Event Creation**
   - Add general information about incidents (description, time, risk level)
   - Set distribution level for sharing across the MISP network
   - Use templates for structured data entry (e.g., Phishing Email category)

2. **Distribution Options**
   - **Your organization only**: Restricted to members of your organization
   - **This Community-only**: Visible to organizations on this MISP server
   - **Connected communities**: Shared with organizations two hops away
   - **All communities**: Freely propagated across all MISP servers
   - **Sharing groups**: Custom predefined list of organizations

3. **Attributes & Attachments**
   - Manual addition or import through formats like OpenIOC
   - Optional IDS flagging for use in detection systems
   - Batch import support for multiple attributes
   - File attachment support (malware, reports, artifacts)
   - Malware files are automatically password-protected

4. **Publishing**
   - Organization admin reviews and publishes events
   - Published events join the pool according to distribution settings

## Feeds and Threat Intelligence
- **Purpose**: Import indicators for attributed security events
- **Benefits**:
  - Exchange threat information continuously
  - Preview events with associated attributes
  - Select and import relevant events
  - Correlate attributes between different sources
- **Management**: Enabled by Site Admin for analyst use

## Taxonomies and Classification
- **Definition**: Standard classification system for threat information
- **Uses**:
  - Set events for external processing (e.g., VirusTotal)
  - Ensure proper classification before publishing
  - Enrich IDS export values with deployment-specific tags
- **Structure**: Machine tags with namespace, predicate, and value
- **Access**: Listed under Event Actions tab, enabled by site admin

## Tagging Best Practices
- **Tag Inheritance**: Tags set at event level cascade to attributes
- **Recommendation**: Tag entire events, use attribute-level tags only for exceptions
- **Essential Tags**:
  - **Traffic Light Protocol (TLP)**: Color schema guiding information sharing
  - **Confidence**: Indicates data quality and vetting status
  - **Origin**: Describes information source (automated or manual)
  - **Permissible Actions Protocol (PAP)**: Defines how data can be used for compromise detection

## Analyst Workflow
- Log in to analyst account to access dashboard
- Create events based on investigations
- Add relevant attributes and file attachments
- Apply appropriate taxonomies and tags
- Submit for organization admin review and publishing
- Correlate with existing events and feeds
- Share according to distribution settings

## Security Considerations
- Malware files are automatically zipped and password-protected
- Distribution settings control information sharing boundaries
- Tagging ensures proper handling of sensitive information
- Input filters provide validation to maintain data quality
